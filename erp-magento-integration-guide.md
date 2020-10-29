
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

## API Reference

The integration to Magento is bi-directional. The backend systems acts as both a source and a target system. This implies that there two forms of logic that are required:

 1. A framework for processing outbound data
	 The expectation is that this should support pushing data on-demand as well as in real-time such as when a customer record has been updated. Please refer to the Connector Integration Guide for more details on how to interact with the connector. 
2. API(s) for processing inbound data
	All inbound data and inbound data request need to have an API to handle and respond to the request. There are two strategies that can be used and a choice between any of them can be made:
	 - Single Ingest API (recommended)
	 The single API is what is recommended. It is an ordechestration or proxy API that uses the metadata of the request payload to delegate to internal APIs that process specific objects
	 
	 - Multiple Object-Specific APIs
	 With this strategy, each of the objects for replication or data request would need to have their own API. This is not dynamic in nature and therefore not the recommended approach

### Request Body Formatting
As outlined in the Connector Integration Guide,  both the outbound and inbound request body is formatted as follows:

    {
      "metadata": "{}",
      "data" : "[]/{}"
    }

The above JSON is for illustration purposes .

#### Metadata 
The metadata describes the object, action and systems in action. This should be constructued from the object names and actions listed in API Data Reference for the corresponding API

#### Data 
The data node can either be an Object or an Array. This means that you can one or more records for an object as needed ( with a single object sent as an object or a single entry in an array). As should be known already

### Response Construction 


## API Data Reference
The following is a listing of all the data expected to and from backend systems.  From this information, you should get the following:
- the object names and actions will form part of the metadata in the request body 
- the data structures will form part of the data node in the request body

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
eyJoaXN0b3J5IjpbLTQ2NDMxODU1MywxOTc5MTc1OTU3LDE3Mj
c4MTczNSwtMTIzNjQzMDE2OSw2Njg2Mzg3NzUsLTIwNTcwNTE2
NTEsMjAwODk4NTY1MSwxMTQyMjIzNDUsNzEwMjEzMDk2LC0xMT
E2Njc2ODU2LDEyMDY0MzQ2MDcsLTQyMDE0MTk0NSwtMjAxMTcz
MTIzNywtMTY5NjkyNDMwNCwtOTEyMDgyMjcwLC03NjkzMjY0Nz
gsLTg2NzExNzQ5NywyMTI1OTQxODAyLDE0Mzc5MDM0MSwtMzk4
Njc0ODk4XX0=
-->