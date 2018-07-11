# Process graphs

A process graph includes specific process calls, i.e. references to one or more processes including specific values for input arguments similar to a function call in programming. However, process graphs can chain multiple processes. In particular, arguments of processes in general can be again (recursive) process graphs, input datasets, or simple scalar or array values.

## Schematic definition

### Process

A single process in a process graph is defined as follows:

```
<Process> := {
  "process_id": <string>,
  "description": <string>,
  "<ArgumentName>": <Value>,
  ...
}
```
A process MUST always contain a key-value-pair named `process_id` and MAY contain a `process_description`.  It MAY hold an arbitrary number of additional elements as arguments for the process.

`process_id` can currently contain three types of processes:

* Backend-defined processes, which are listed at `GET /processes`, e.g. `filter_bands`.
* User-defined process graphs, which are listed at `GET /users/{user_id}/process_graphs`. 
  They are prefixed with `/user/`, e.g. `/user/my_process_graph`.
* User-defined functions (UDF) are prefixed with `/udf` and additionally contain the runtime and the process name separated by `/`, e.g. `/udf/Python/apply_pixel`.

**Arguments**

A process can have an arbitrary number of arguments.

The key `<ArgumentName>` can be any valid JSON key, but it is RECOMMENDED to use [snake case](https://en.wikipedia.org/wiki/Snake_case) and limit the characters to `a-z`, `0-9` and the `_`. `<ArgumentName>` MUST NOT use the names `process_id` or `process_description` as it would result in a naming conflict.

A value is defined as follows:

```
<Value> := <string|number|array|boolean|null|Process>
```

*Note:* string, number, array, boolean and null are the primitive data types supported by JSON. An array MUST always contain *one data type only* and is allowed to contain the data types allowed for `<Value>`. In consequence, the objects allowed to be part of an array are processes only.

*Note:* The expected names of arguments are defined by the process descriptions, which can be discovered with calls to `GET /processes` and `GET /udf_runtimes/{lang}/{udf_type}`. Therefore, the key name for a key-value-pair holding an image collection as value doesn't necessarily need to be named `imagery`. The name depends on the name of the corresponding process argument the image collection is assigned to. Example 2 demonstrates this by using `collection` as a key once. 

### Examples

**Example 1:** A full process graph definition.

```
{
  "process_id":"min_time",
  "imagery":{
    "process_id":"/udf/Python/custom_ndvi",
    "imagery":{
      "process_id":"filter_daterange",
      "imagery":{
        "process_id":"filter_bbox",
        "imagery":{
          "process_id":"get_data",
          "data_id":"S2_L2A_T32TPS_20M"
        },
        "left":652000,
        "right":672000,
        "top":5161000,
        "bottom":5181000,
        "srs":"EPSG:32632"
      },
      "from":"2017-01-01",
      "to":"2017-01-31"
    },
    "red":"B04",
    "nir":"B8A"
  }
}
```

**Example 2:** If a process needs multiple processes as input, it is allowed to use arrays of the respective types.

```
{
  "imagery":{
    "process_id":"union",
    "collection":[
      {
        "process_id":"filter_bands",
        "imagery":{
          "process_id":"get_data",
          "data_id":"Sentinel2-L1C"
        },
        "bands":8
      },
      {
        "process_id":"filter_bands",
        "imagery":{
          "process_id":"get_data",
          "data_id":"Sentinel2-L1C"
        },
        "bands":5
      }
    ]
  }
}
```