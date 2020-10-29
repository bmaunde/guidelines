
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

## Data and Actions
The following is a listing of all the data expected to and from backend systems.  The objects and actions information helps in the composition of the metadata that is included in the request body.

### Outbound Data
This section covers data that is to be replicated from backend systems to Magento

#### Customers
##### Object Names
For B2C scenarios, the expected object is:
-  **CONSUMER** for all the customers who will be buying on the e-commerce site'

For B2B scenarions, the expected objects are:
-  **CONSUMER** for the contact persons associated with a company. These will act on behalf of the company. 
- **CUSTOMER** for the companies. 

##### Actions 
The supported actions are:
- **CREATE**  - with this action, the contained data will be created in Magento
- **UPDATE**  - with this action, the contained data will be updated in Magento
- **DELETE**  - with this action, the contained data will be created in Magento

##### Data Structures
###### Consumer 
###### Customer

#### Products
This relates to the products/materials and services that will be 
##### Object Names
The name of the object is **PRODUCT**

##### Actions 
The supported actions are:
- **CREATE**  - with this action, the contained data will be created in Magento
- **UPDATE**  - with this action, the contained data will be updated in Magento
- **DELETE**  - with this action, the contained data will be created in Magento

##### Data Structures

#### Pricing
This relates to basic and advanced pricing for products. 
##### Object Names
The name of the object is **PRICE**

##### Actions 
The supported actions are:
- **CREATE**  - with this action, the contained data will be created in Magento
- **UPDATE**  - with this action, the contained data will be updated in Magento
- **DELETE**  - with this action, the contained data will be created in Magento

##### Data Structures
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjc4OTkxMiwtMjAxMTczMTIzNywtMTY5Nj
kyNDMwNCwtOTEyMDgyMjcwLC03NjkzMjY0NzgsLTg2NzExNzQ5
NywyMTI1OTQxODAyLDE0Mzc5MDM0MSwtMzk4Njc0ODk4LDEzND
kwNzU5NSwtMTA0MTc0NDcxOF19
-->