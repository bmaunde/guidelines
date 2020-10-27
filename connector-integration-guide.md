
# Connector Integration Guide

## Overview

The Consnet Connector, a Consnet service, is a middleware-as-a-service environment where any supported systems can be connected to one another for the purposes of sharing data in synchronous manner. The data involved can be anything ranging from persisted master data to ad-hoc events.

Use cases for Consnet Connector include, but are not limited to:

- Transfer of master data to another system for persistence
- Retrieval of data from another system for use during the execution of a business process
- Sending an asynchronous event to another system

Examples of possible integrations include, but are not limited to:

- Integrating an ERP system to an E-Commerce system for the sharing customer, product, pricing, and order data

- Integrating an E-Commerce system to a reporting system for event recording and reporting

## Integration 

### Supported Integration Models

The Connector supports both Point-to-Point (p2p) and Middleware (broadcast) integration models.

Note that the broadcasting of messages is currently supported when the adapter design guideline for integrating to the Consnet Connector is used. In future, broadcasting will be supported within the connector itself.

### Supported Processing Strategies

The Consnet Connector currently supports both the Batch and Realtime integration strategies.

### Supported Protocols

The Connector exposes a REST based API that accepts HTTP requests. It also expects that the recipient systems to which data will be forwarded expose a REST API to receive and process data. 

Other methods of integration may be supported in the future.

### Prerequisites

Before using the Connector, the following information will need to be registered:

- user’s information
- user’s systems to be integrated.
- routing information regarding the source and target systems
- receiving APIs in the target system – note that only REST based APIs are supported
- authentication details for all – note that only BASIC, Token, and OAuth 2 are the only supported authentication methods

Upon registration, OAuth credentials ( client and secret ) are provided for use when sending requests to the Connector.

### Data Integration API

A single REST based data ingesting API, to which source systems send data, is exposed . The API is located at either:

Sandbox: https://cdp-dev.cncorp.co.za/gdc/api/connector/v1
Production: https://cdp.consnet.co.za/gdc/api/connector/v1

 #### Endpoint
 
The data API is accessed at the following endpoint:

> /data

#### Supported Methods

Only the **POST** method is currently supported. The use of any other method will result in 405 (method not supported) errors being returned.

#### Supported Data Formats

Only **JSON** is currently supported. Therefore, the Content-Type header field must be populated with **application/json** as shown below:

> Content-Type: application/json_

In future, other formats may be supported

#### Authorization

Only **OAuth 2** is supported for authorization.

##### Token Retrieval

Before sending a request, a system needs to acquire a bearer access token from the authorization API using the credentials provided upon registration.

The request for the retrieval of the access should be a REST call to the following endpoint, depending on your environment:

> Sandbox: [https://cdp-dev.cncorp.co.za/uaa/oauth/token](https://cdp-dev.cncorp.co.za/uaa/oauth/token)
> Production: [https://cdp.consnet.co.za/uaa/oauth/token](https://cdp.consnet.co.za/uaa/oauth/token)

The Grant Type for the OAuth access token request should be **client_credentials**. It is advisable to store the token and re-use it until expiry to avoid making too many requests to the authorization API.

##### Authorization Header

With the access token retrieved, all data API requests should have an authorization header with the Bearer authentication scheme maintained as shown below:

> Authorization: Bearer \<Access-Token>

#### Request Body

As indicated earlier, the only supported format is JSON. As such, the request body and the corresponding header should be consistent.

The request body must have 2 nodes, namely:
- metadata, and
- data

The below illustatrates the structure

    { "metadata":"<metadata>","data" : "<payload>" }

##### Metadata
The metadata nodes is a data structure that has the following nodes:

 - object  - this represents the description of the data being sent. This needs to be registered in the Connector. Examples are order, customer, and product among others
 - action - this represents the operation that the receiving system will perform on the payload being sent. Examples are create, update, delete, remove, and validate among others
 - route - this is a data structure that describes the route the data has to take. It identifies the source, target system and format of data. The data structure has the following nodes in it
	 - source - this represents the id of the source system
	 - target - this represents the id of the target system
	 - format - this represents the format of the data payload. Currently, the supported value for this is **JSON**
- integrationKey - this is a unique key that you should generate that identifies your requests. We suggest that the key should be composed as follows: 
	>\<uuid/guid>+\<timestamp>+\<randomly generated number with a minimum length of 6 digits>

All the information in the metadata data structure is part of the system or routuing information that is registered with the Connector before testing the integration. If the information does not match the configuration in the Connector, the request will be denied and an error is returned in the response.

An illustration of the metadata node is given below:

    "metadata": { "integrationKey": "012345678915998258317381780","action": "CREATE","object": "ORDER","route": { "source": "testsource","target": "testtarget","format": "JSON"}}

##### Data
The data node contains the actual payload for the data to be exchanged between systems. This can be an object or an array
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYxNjUzMjAxNCwtMjExMzYyNTU0NSwyMD
ExNDY4MTUxLDgxNjkwNjE1MiwtMTc4OTUzOTk5Nl19
-->