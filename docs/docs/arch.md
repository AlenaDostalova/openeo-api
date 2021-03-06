# Architecture

The openEO API defines a language how clients communicate to back-ends in order to analyze large Earth observation datasets. The API will be implemented by drivers for specific back-ends. Some first architecture considerations are listed below.

1. The openEO API is a contract between clients and back-ends that describes the communication only
2. Each back-end runs its own API instance including the specific back-end driver. There is no API instance that runs more than one driver.
3. Clients in R, Python, and JavaScript connect directly to the back-ends and communicate with the back-ends over HTTP(s) according to the openEO API specification.
4. API instances can run on back-end servers or additional intermediate layers, which then communicate to back-ends in a back-end specific way.
5. Back-ends may add functionality and extend the API wherever there is need.
6. There will be a central back-end registry service, to allow users to search for back-ends with specific functionality and or data. 
7. The openEO API will define _profiles_ in order group specific functionality.

![Architecture](arch.png)


# Microservices

To simplify and structure the development, the API is divided into a few microservices.

| Microservice               | Description                                                  |
| -------------------------- | ------------------------------------------------------------ |
| Capabilities               | This microservice reports on the capabilities of the back-end, i.e. which API endpoints are implemented, which authentication methods are supported, and whether and how UDFs can be executed at the back-end. |
| EO Data Discovery          | Describes which datasets and image collections are available at the back-end. |
| Process Discovery          | Provides services to find out which processes a back-end provides, i.e., what users can do with the available data. |
| UDF Runtime Discovery      | Allows discovering the programming languages and their runtime environments to execute user-defined functions. |
| Job Management             | Organizes and manages jobs that run processes on back-ends.  |
| Result Access and Services | Services to download data and job results, e.g. as WCS or WMTS service. |
| User Data Management       | Manage user content and accounting. Might be split into multiple microservices, e.g. User Files and User Process Graphs might be separated. |
| Authentication             | Authentication of users.                                     |
