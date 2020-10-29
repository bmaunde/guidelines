
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
-  **CONSUMER** for the contact persons associated with a company. These will act on behalf of the company. 
- **CUSTOMER** for the companies. 

##### Actions 
The supported actions are:
- **CREATE** 
- **UPDATE** 
- **DELETE** 

##### Data Structures
###### Consumer 
###### Customer

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

#### Orders
When orders in the backend system are needed in Magento, the following information can be used. This is an optional

##### Object Names
The name of the object is **PRICE**

##### Actions 
The supported actions are:
- **CREATE**  
- **UPDATE**  
- **DELETE** 

##### Data Structures


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

#### Orders
When orders created in Magento are needed in backend systems, their replication to backend systems is supported. 

##### Object Names
The object name is **ORDER** 

##### Actions 
The supported actions are:
- **CREATE** 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzA1MjY0MTc1LDcxMDIxMzA5NiwtMTExNj
Y3Njg1NiwxMjA2NDM0NjA3LC00MjAxNDE5NDUsLTIwMTE3MzEy
MzcsLTE2OTY5MjQzMDQsLTkxMjA4MjI3MCwtNzY5MzI2NDc4LC
04NjcxMTc0OTcsMjEyNTk0MTgwMiwxNDM3OTAzNDEsLTM5ODY3
NDg5OCwxMzQ5MDc1OTUsLTEwNDE3NDQ3MThdfQ==
-->