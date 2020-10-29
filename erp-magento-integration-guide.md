
# Magento Commerce Accelerator Integration Guide

## Versioning 

### Version 
1.0 

### What's New
Intentionally left blank

## Target Audience
Technical experts who intent to integrate backend systems to Consnet Magento Accelerator based installations using the Consnet Connector.

## Related Documents
For information on how to integrate systems using the Consnet Connector, please refer to the Connector Integration Guide.

## Overview
The Consnet Magento Accelerator enables any Magento Commerce installation based on it to be ready for integration with backend system for various master data and transactional objects. 

The purpose of this document is to provide information on:

 - the objects from Magento to backend systems
 - the objects from backend systems to Magento
 - the actions against the objects
 - the data structures expected in Magento for the objects 
 - The data structures sent from Magento for the object

The information mentioned above will help in the the development of the following in the backend systems: 

 - the push mechanisms for the data expected from backend systems
 - the persistence/handling mechanisms for data pushed from Magento

## Requirements: Sync Framework & API Reference

The integration to Magento is bi-directional. The backend systems acts as both a source and a target system. This implies that there two forms of logic that are required:

 1. A synchronization framework for processing outbound data
	 The expectation is that this should support pushing data on-demand as well as in real-time such as when a customer record has been updated. Please refer to the Connector Integration Guide for more details on how to interact with the connector. 
2. API(s) for processing inbound data
	All inbound data and inbound data request need to have an API to handle and respond to the request. There are two strategies that can be used and a choice between any of them can be made:
	 - Single Ingest API (recommended)
	 The single API is what is recommended. It is an ordechestration or proxy API that uses the metadata of the request payload to delegate to internal APIs that process specific objects
	 
	 - Multiple Object-Specific APIs
	 With this strategy, each of the objects for replication or data request would need to have their own API. This is not dynamic in nature and therefore not the recommended approach

### Synchronization Framework 
The frmework is expected to facilitate the real-time and on-demand replication of the following:

 1. Customers
	 The sychronization framework should be able send customer data. In a B2C scenario, this is just the consumers in the backend system. In a B2B scenario, this refers to the companies in the backend system.
2. Products 
	The sychronization framework should be able to send product or service data from the backend system
3. Pricing
    Pricing information is required so that Prices can be displayed on the store. The sychronization framework should be able to send pricing information for product replicated to Magento
 4. Orders
	 If orders are required to be replicated to Magento. Therefore the synchronization framework should support this, if needed.

### Inbound API(s)
Regardless of the choice inbound processing API - whether a Single Ingest API or Multiple Object-Specific APIs - the following inbound data or requests should be supported:

 1. Order Processing
	 This is required to recieve and process orders sent from Magento. This API should return a response as well. For more information, refer to the API Data Reference. 
2. Customer Processing 
	This is only relevant if replication of customers created in Magento is necessary. If so, this API should process inbound customer data processing requests. Refer to the API Data Reference for more information 
3. Credit and ATP Check
	This API is important for the purposes of performing Credit and ATP checks while processing orders in Magento. The request structure and the expected response are detailed in the  API Data Reference section 
4. Billing Documents Search and Download 
	This API is used to search for invoices, invoice cancelallations, and debit and credit notes. After the search, the user typically downloads PDF copy of the invoice. The API should support this. Please refer to the API Data Reference section for more information. 
5. Statement Download
	This API allows a customer to download a statement for a particular period (month and year). The request and response are detailed in the API Data Reference section. 
	
## Request Body Formatting
As outlined in the Connector Integration Guide,  both the outbound and inbound request body is formatted as follows:

    {
      "metadata": "{}",
      "data" : "[]/{}"
    }

The above JSON is for illustration purposes . More information on each of the nodes is given below and is also included in the Connector Integration Guide.

The sync framework  making requests to the Connector to send data to Magento should build the request body as above and send the data in JSON format. 
Likewise, the receiving API(s) should be able to read the payload and process it as required.

#### Metadata 
The metadata describes the object, action and systems in action. This should be constructued from the object names and actions listed in API Data Reference for the corresponding API. Please refer to the Connector Integration Guide for more information on how to build this structure.

The metadata is formulated as this:

    {
      "metadata": {
        "action": "<Action>",
        "object": "<Object Name>",
        "integrationKey": "<generated unique key>",
        "route": {
          "source": "<Source System>",
          "target": "<Target System ID>",
          "format": "JSON"
        }
      }
    }

#### Data 
The data node can either be an Object or an Array. This means that you can one or more records for an object as needed ( with a single object sent as an object or a single entry in an array).  When sending multiple records, an array should be used instead of multiple requests. When a single record is being sent, the choice is for the integrator to make as the Connector supports both. 



## Response Body Formatting 
For realtime inbound requests that require a response, the response just needs to be the resultant data structure in JSON format as that is the only currectly supported format.

## Error Handling 
In both the Sync Framework and the Inbound API(s), error handling is expected. The structure of the error data and the response codes that should be used are detailed in the Connector Integration Guide.


## API Data Reference
This section covers the technical information that will be used in the metadata of the request body as well as the data structures expected in the data part of the request body. The response structures for synchronous data request APIs is also covered to help with the creation of the response body. 

Note that the data structures are have example data in them. If you want to understand the meaning of the data structures, please refer to the guide on the data structures or contact your Consnet contact.

### Outbound Data
This section covers data that is to be replicated from backend systems to Magento. This mainly consists of master data and the objects are:

 - Customers
 - Consumers
 - Products/Services
 - Pricing

#### Customers
This relates to the customer who will buy the products and services in Magento. 

##### Object Names
For B2C scenarios, the expected object is:
-  **CONSUMER** for all the customers who will be buying on the e-commerce site'

For B2B scenarions, the expected objects are:
- **CUSTOMER** for the companies. Contact persons are also included as part of the data structure

##### Actions 
The supported actions are:
- **CREATE** 
- **UPDATE** 
- **DELETE** 

##### Data Structures
###### Consumer 
###### Customer with Contact
The structure below contains a contact node, which is used to represent contact persons for a company in a B2B scenario

    {
	    "number": "0000100740",
	    "name": "Sonic Master",
	    "otherName": "Sonic Master",
	    "taxes": [
	      {
	        "number": "6542139870"
	      }
	    ],
	    "address": {
	      "telNumbers": [
	        {
	          "number": "0113213546",
	          "default": true
	        }
	      ],
	      "emailAddresses": [
	        {
	          "emailAddress": "sonicmaster99@yopmail.com",
	          "default": true
	        }
	      ],
	      "street": "12Peltier Drive",
	      "city": "Johannesburg",
	      "country": "ZA",
	      "region": "GP",
	      "postalCode": "2152",
	      "externalId": "0000694626"
	    },
	    "salesAreas": [
	      {
	        "salesOrg": "3020",
	        "disChannel": "30",
	        "division": "00",
	        "customerGroup": "10",
	        "priceGroup": "03",
	        "shippingConditions": "02",
	        "paymentTerms": "0001"
	      }
	    ],
	    "contacts": [
	      {
	        "title": "Mr.",
	        "firstName": "Sonic",
	        "lastName": "Adams",
	        "sex": "1",
	        "email": "sonicadams99@yopmail.com",
	        "address": {
	          "telNumbers": [
	            {
	              "number": "0113213512",
	              "default": true
	            }
	          ],
	          "emailAddresses": [
	            {
	              "emailAddress": "sonicadams99@yopmail.com",
	              "default": true
	            }
	          ],
	          "street": "12Peltier Drive",
	          "city": "Johannesburg",
	          "country": "ZA",
	          "region": "GP",
	          "postalCode": "2152"
	        },
	        "contactFunction": "EA",
	        "customerId": "0000153505"
	      }
	    ] 
    }

#### Products
This relates to the products/materials and services that will be used in Magento

##### Object Names
The name of the object is **PRODUCT**

##### Actions 
The supported actions are:
- **CREATE**  
- **UPDATE** 
- **DELETE** 

##### Data Structures

    {
	    "sku": "PROD5008",
	    "name": "Cool Product 5L",
	    "description": "<P>For use on interior surfaces only</P>",
	    "categoryMapping": "002",
	    "catalogMapping": [
	      {
	        "salesOrg": "3020",
	        "disChannel": "30"
	      }
	    ]
    }

#### Pricing
This relates to basic and advanced pricing for products. 

##### Object Names
The name of the object is **PRICE**

##### Actions 
The supported actions are:
- **CREATE**  
- **UPDATE**  
- **DELETE** 

##### Data Structures

    {
      "salesOrg": "3020",
      "disChannel": "30",
      "product": "MB100",
      "price": 800,
      "quantity": 1,
      "customerGroup": "0000100733",
      "priceType": "ZAR",
      "validfrom": "2020-10-27",
      "validto": "9999-12-31",
      "advancedPricing": [
        {
          "quantity": 1,
          "value": 800
        },
        {
          "quantity": 3,
          "value": 775
        }
      ]
    }

#### Orders
When orders in the backend system are needed in Magento, the following information can be used.

##### Object Names
The name of the object is **ORDER**

##### Actions 
The supported actions are:
- **CREATE**  
- **UPDATE**  

##### Data Structures
An optional response to the synchronous call for the creation of the order may return a response in the form given in the response section below. 

###### Request 
    {
      "customer": "0000100744",
      "currency": "ZAR",
      "externalid": "0006100154",
      "orderdate": "2020-10-29",
      "ponumber": "MB201",
      "ordernumber": "006100154",
      "orderstatus": "B",
      "userstatus": "IN_PROGRESS",
      "shippingaddress": "0000694646",
      "pricing": {
        "net": 139.81,
        "gross": 139.81
      },
      "items": [
        {
          "id": "1828",
          "externalid": "000010",
          "product": "C0802",
          "quantity": 1,
          "status": "A",
          "pricing": {
            "net": 13.45,
            "gross": 13.45,
            "discount": ""
          },
          "schedulelines": [
            {
              "deliverydate": "2020-11-05",
              "unit": "PC"
            }
          ]
        }
      ]
    }

###### Response 


### Inbound Data
This section covers data that is expected from Magento. This mainly consists of transactional data. For some scenarios, there could be a need to also replicate customers created in Magento to the backend systems.  The obects covered are:

 - Customers
 - Orders

#### Customers 
This is in an **optional** scenario where customer created in Magento are needed in the backend systems for downstream processes like order processing or accounting. 

##### Object Names
For B2C scenarios, the expected object is:
-  **CONSUMER** for all the Magento customers

For B2B scenarions, the expected objects are:
-  **CONSUMER** for all the Magento customers associated with a Magento company. These will act on behalf of the company. 
- **CUSTOMER** for the Magento companies. 

##### Actions 
The supported actions are:
- **CREATE** 
- **UPDATE** 
- **DELETE** 

##### Data Structures

#### Orders
When orders created in Magento are needed in backend systems, their replication to backend systems is supported. 

##### Object Names
The object name is **ORDER** 

##### Actions 
The supported actions are:
- **CREATE** 

##### Data Structures

     {
      "header": {
        "externalid": "006100152",
        "documentdate": "20201029",
        "ponumber": "Test",
        "customer": "0000100720",
        "currency": "ZAR",
        "externalcreator": "douwess99@yopmail.com",
        "salesorg": "3020",
        "dischannel": "30",
        "division": "00"
      },
      "items": [
        {
          "externalid": "1814",
          "product": "C8060",
          "quantity": 30,
          "unit": ""
        }
      ]
    }

### Inbound Realtime Requests
There are instances when Magento requests information. There are currently 3 scenarios in which this is the case and they are:

 - Credit and ATP check 
 - Biling Document Search and Download
 - Statement Download
 - Order creation - for details on this 

#### Credit and ATP Check
This is a request made to the backend systems for credit and atp information. This involves a Credit Check and ATP check according to the backend system's functionality . 

##### Object Name
The object name is **ORDER** 

##### Actions 
The supported actions are:
**CREDITATP** - combined call for ATP and Credit Check

##### Data Structure

###### Request
    {
      "credit": {
        "amount": 2805.69,
        "currency": "ZAR"
      },
      "atp": [
        {
          "product": "TEST005",
          "quantity": 1,
          "unit": null,
          "deliverydate": null
        }
      ],
      "sales": {
        "salesorg": "3020",
        "dischannel": "30",
        "division": "00"
      },
      "customer": "0000100729",
      "ordertype": "ZZOR"
    }

###### Response 

    {
      "credit": {
        "passed": true,
        "available": 6666.67,
        "limit": 6666.67
      },
      "atp": [
        {
          "passed": true,
          "product": "TEST005",
          "available": 45,
          "unit": "EA"
        }
      ]
    }

#### Billing Documents
This invoices billing documents in the form of invoices, debit notes and credit notes. A request for billing documents is made in 2 steps which are: 

 1. Search - returns a list of billing documents for a customer
 2. Download - returns the base64 encoded PDF content of a billing document

for the search functionality as well as the download functionality. The search action should returnsa list of invoices, whereas a download action should return . 

##### Object Name 
The object name is **BILLINGDOCUMENT**

##### Actions 
The supported actions are:
**SEARCH**  - for searching for billing documents
**DOWNLOAD** - for downloading a specific billing document

##### Data Structures
The data structures here represent the request data payload as well as the response structure that should be returned as a response to the request. Remember that the response body does not need to contain metadata.

###### Search Request

    {
      "customer": "0000008833",
      "maxrows": 1000,
      "startdate": "",
      "enddate": "",
      "salesorg": "",
      "dischannel": "",
      "division": "",
      "companycode": ""
    }
###### Search Response 

    [
      {
        "id": "0091643242",
        "type": "Invoice",
        "customer": "0000008833",
        "payer": "0000008833",
        "billingdate": "2017-04-10",
        "paymentterms": "Z1D",
        "paymenttermsdesc": "Payable 30 days 7.5% disc debt",
        "reference": "3005357807",
        "netvalue": 3371,
        "tax": 471.94,
        "currency": "ZAR"
      }
    ]

###### Download Request 

    {
      "id": "0091705860"
    }
###### Download Response 

    {
      "billdoc": "0091705860",
      "pdf": "",
      "length": 0
    }

#### Statements 
This is useful for statement downloads. The response should return a base 64 encoded PDF content

##### Object Name 
The object name is **STATEMENT**

##### Actions 
The supported actions are:
**DOWNLOAD**

##### Data Structures
The data structures here represent the request data payload as well as the response structure that should be returned as a response to the request. Remember that the response body does not need to contain metadata.
###### Request 

    {
      "customer": "CMS0000010",
      "salesorg": "3000",
      "month": "10",
      "year": "2020"
    }

###### Response 

    {
      "year": "2020",
      "month": "10",
      "pdf": "",
      "length": 0
    }

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzOTkxMzE5MzEsODYxNzYwMDc5LC0xMD
IxMDkwMDcxLC0zNjg1OTEwNDksMjE5MjM0MDAyLC0xNzU1NjU2
MDM2LC00ODcyODE0NjQsLTk1ODk4MzQyOSwtMjE0NzExOTQ5Mi
wtMTkxMDI4ODcyMCwxOTc5MTc1OTU3LDE3Mjc4MTczNSwtMTIz
NjQzMDE2OSw2Njg2Mzg3NzUsLTIwNTcwNTE2NTEsMjAwODk4NT
Y1MSwxMTQyMjIzNDUsNzEwMjEzMDk2LC0xMTE2Njc2ODU2LDEy
MDY0MzQ2MDddfQ==
-->