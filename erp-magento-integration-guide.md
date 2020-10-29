
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

## Data Reference
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
 - Invoice Search and Download
 - Statement Download

#### Credit and ATP Check
This is a request made to the backend systems for credit and atp information. This involves a Credit Check and ATP check according to the backend system's functionality . 

##### Object Name
The object name is **ORDER** 

##### Actions 
The supported actions are:
**CREDITATP** - combined call for ATP and Credit Check

##### Data Structure

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

#### 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MjIzNzI0NDksNjY4NjM4Nzc1LC0yMD
U3MDUxNjUxLDIwMDg5ODU2NTEsMTE0MjIyMzQ1LDcxMDIxMzA5
NiwtMTExNjY3Njg1NiwxMjA2NDM0NjA3LC00MjAxNDE5NDUsLT
IwMTE3MzEyMzcsLTE2OTY5MjQzMDQsLTkxMjA4MjI3MCwtNzY5
MzI2NDc4LC04NjcxMTc0OTcsMjEyNTk0MTgwMiwxNDM3OTAzND
EsLTM5ODY3NDg5OCwxMzQ5MDc1OTUsLTEwNDE3NDQ3MThdfQ==

-->