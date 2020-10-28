
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
The Connector uses a 3 stage processing model for any data that it integrates. The 3 stages are; 

 1. Ingest - this is where data is accepted and validated for consistency with configuration
 2. Transform - this is where the data payload is transformed into a structure expected by the target system. This allows for the same data to be sent to disparate systems
 3. Publish - this is where data is sent to the target system, and the corresponding response from the target system processed, before a response to the source system is sent

### Supported Integration Models

The Connector supports both Point-to-Point (p2p) and Middleware (broadcast) integration models.

Note that the broadcasting of messages is currently supported when the adapter design guideline for integrating to the Consnet Connector is used. In future, broadcasting will be supported within the connector itself.

### Supported Processing Strategies

The Consnet Connector currently supports both the Batch and Realtime integration strategies.

### Supported Protocols

The Connector exposes a REST based API that accepts HTTP requests. It also expects that the recipient systems to which data will be forwarded expose a REST API to receive and process data. 

Other methods of integration may be supported in the future.

### Prerequisites for Integration 

#### Registration 
Before using the Connector, the following information needs to be registered:

- user’s information
- user’s systems to be integrated.
- routing information regarding the source and target systems
- receiving APIs in the target system – note that only REST based APIs are supported. Authentication information for the APIs should also be provided. Note that only BASIC, Token, and OAuth 2 are the only supported authentication methods for APIs the Connector can call.
- transformation specifications - this is based on a JSONATA specification. This enables the tranformation stage where data is transformed from the source structure or a structure expected by the target systems.

#### Credentials
Upon registration, OAuth credentials ( client and secret ) are provided for use when sending requests to the Connector.

#### SSL 
The Connector's integration endpoints are SSL protected. Your integration logic needs to be able to work with HTTPS. 

### Integration Roles
A system that plays a role in the Connector-based integration can play either or both of two roles:

 - Source System
 - Target System
 
 ####  Source System 
 A source is responsible for sending or sourcing data from other systems. It is the initiator. When integrating as a source system,  the system needs to be able to:
 -  Send authorization and data integration requests to the Connector's processing API
 -  Correctly create the request body required by the Connector.
 -  Process the response returned by the Connector 
 -  Communicate securely using the HTTPS protocol
 
 More information on the request, request body and response structuring is given in the API Reference  section 

#### Target System 
A target system processes data sent from the source systems via the Connector. Traget systems need to be able to:
- Receive, at an exposed endpoint, data from the connector 
- Process the data as specified in the metadata 
- Return a response indicating success or failure with a corresponding response body
- Communicate securely over the HTTPS protocol

More information on the request, request body and response structuring is given in the API Reference  section 

## API Reference

### API Requests

A single REST based data processing API, to which source systems send data, is exposed . The API is located at either:

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

    { 
      "metadata":"<metadata>",
      "data" : "<payload>" 
    }

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

    "metadata": { 
    "integrationKey": "012345678915998258317381780",
    "action": "CREATE",
    "object": "ORDER",
    "route": { "source": "testsource",
			   "target": "testtarget",
			   "format": "JSON"
			  }
	}

##### Data
The data node contains the actual payload for the data to be exchanged between systems. This can be an object or an array. It can even be a simple value.

The data payload will be transferred as is or will be mapped to a target data structure understood by the target system within the connector. See the data mapping section.

#### Response Format
The only format that is supported at the moment is JSON. It is there important that an **Accept** header value be set to **application/json** when making a request to Connector. 

> Accept: application/json

### API Response
The Connector's data processing API returns a response for all requests made to it. The response is in 2 parts:

- HTTP status Code 
- JSON response body containing data/error payloads

#### Status Codes
In general, the following HTTP status codes are returned:

 - For successful requests a **2XX** status code is returned
 - For failed requests, either a **4XX** or a **5XX** status code is returned. 
 
The following are the common status codes to be expected in the response:

| Status Code  | Explanation |
|--------|-----------------|
| 200 | The request was successfully processed by the target system |
| 201 | The request was successfully processed by the target system |
| 3XX | Moved, the URL has changed |
| 400 | Bad request. The request sent is malformed |
| 401 | Unauthorized. This means the authorization header field has not been included with the request or the value thereof is invalid |
|403 | Access to a resource against which no authorization has been given |
| 404 | An incorrect URL was specified for the endpoint |
| 405 | Only the **POST** is currently supported. This means a different HTTP method has been used |
| 500 | An error has occured whilst processing the request in the **Connector** |
| 502 | An error has occured whilst processing the request in the **target** system |


#### Response Body
The response body is a JSON formatted data payload. There are two types of data contained in this payload:

 - A payload containing whatever data the target system has decided to send back.  Examples of this are:
	 - a success message 
	 - a resource that was created due to a create action on an object
	 - a resource that was retrieved due to a get/retrieve/read action on an object
	 - a message regarding a pull request such as available credit for a customer
	 
- A payload containing an error message due to a failure in the Connecter or target system. This is the case only when the HTTP status code returned is a **4XX** or a **5XX**. 

##### Error Payload 
The error paylod is always provided in the following format.

    {

        "message": "Could not process data",
        "statusCode": 500,
        "reason": "Internal Server Error",
        "type": null,
        "errors": [
         {
	         "message": "Could not process the provided metadata",
	         "statusCode": 0,
	         "reason": null,
	         "type": null,
	         "errors": null
	    }
	   ]
    }

The above data structure is recursive but it usually 2 levels deep. The first level represents the overral message regarding the error that has occured. The second level provides more detail with regards to what actually caused the error to occur. 

## Target API Guidelines
One of the stages in the 3 staged model of processing for the Connector is the Publishing stage. This is where the data or request from source systems are forwarded to target systems. For this to be successful, an API is registered within the connector to receive the requests/data. 

Below is a specification of what is expected from the API. 

### API Type
The receiving API must be REST based as that is the only mechanism currently supported by the Connector. Other types may be supported in the future

### Authorization
The supported target authorization schemes are Basic and OAuth. More may be supported in the future

### Security 
It is expected that the endpoint be accepted over an SSL protected connection

### Request Body 
The content type for the request to target systems is currently **application/json**. 

The request body  is an object with 2 nodes - metadata and data. The receiving API should be able to read the metadata to find out the object type, the action to be performed and the source system for the data payload. 

Below is the structure of the request body:
   
     { 
          "metadata":"<metadata>",
          "data" : "<payload>" 
     }
 
 The metadata structure is explain in the API reference section above. Please refer to it.

### Response 
Once the data has been processed as expected, a response should be returned. The response has 2 parts:

 - HTTP status code
 - Response payload 

#### Status Codes 
With the status codes, please reference the expected status codes and their meaning:
-  **2XX** codes should be used when a request has been processed successfully
- **4XX** codes should be used for client errors such as formatting or payload structuring errors
- **5XX** codes should be used to indicate an error processing the request

The following are the status codes expected in the response:

| Status Code  | Explanation |
|--------|-----------------|
| 200 | The request was successful |
| 201 | The request was successful |
| 3XX | Moved, the URL has changed |
| 400 | Bad request. The request sent is malformed |
| 401 | Unauthorized. This means the authorization header field has not been included with the request or the value thereof is invalid |
|403 | Forbidden. Access to a resource against which no authorization has been given |
| 404 | An incorrect URL was specified for the endpoint |
| 405 | Only the **POST** is currently supported. This means a different HTTP method has been used |
| 500 | An error has occured whilst processing the request |



#### Response Payload
Response Payloads are currently only expected to be in JSON format. The expected content-type is, therefore, **application/json**

If the processing of a request was successful, the HTTP status code is sufficient. If the request is for retrieval of data, then the payload will contain the requested data. 

In case of an error, the following payload format is expected. Note that the structure is recursive. You are expected to have, at most, 2 levels. 

    {

        "message": "",
        "statusCode": 000,
        "reason": "",
        "type": "",
        "errors": [
         {
	         "message": "",
	         "statusCode": 000,
	         "reason": "",
	         "type": "",
	         "errors": []
	    }
	   ]
    }
   


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY4OTE5NDEwMiwtOTIwNTM3NTI5LDEyMD
g0NDU1NTYsLTc0MTI1Nzc3OCw4MjEwNTE2MDgsLTc5NzE5NDkx
NCwtMTkzOTIzNjIyNiwtMTg1MjgxMDkwMywtNTE0Mzg0MTI4LD
E2MTY1MzIwMTQsLTIxMTM2MjU1NDUsMjAxMTQ2ODE1MSw4MTY5
MDYxNTIsLTE3ODk1Mzk5OTZdfQ==
-->