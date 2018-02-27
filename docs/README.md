
# Generation of OpenEO API documentation

[![Status](https://img.shields.io/badge/Status-proof--of--concept-yellow.svg)]()


This Docker image combines static and auto-generated API documentation of the [OpenEO API](https://github.com/Open-EO/openeo-api-poc/). 
You can see the resulting documentation at [https://open-eo.github.io/openeo-api-poc](https://open-eo.github.io/openeo-api-poc).

## Build documentation
```
git clone https://github.com/Open-EO/openeo-api-poc
docker build -t openeo-api-docs openeo-api-poc/docs
docker run -v $PWD:/shared openeo-api-docs
# -> resulting documentation is in the site folder
```


## Used tools 
- [Swagger UI](https://github.com/swagger-api/swagger-ui)
- [MkDocs](https://github.com/mkdocs/mkdocs)
- [Docker](https://www.docker.com/)

