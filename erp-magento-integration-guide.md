
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

The information mentioned above will enable the development of the following in the backend systems: 

 - the push mechanisms for the data expected from backend systems
 - the persistence/handling mechanisms for data pushed from Magento

## Data and Actions
The following is a listing of all the data expected to and from backend systems. 

### Outbound Data from Backend Systems

#### Customers
##### Object Names
For B2C scenarios, the expected object is:
-  **CONSUMER** for all the customers who will be buying on the e-commerce site'

For B2B scenarions, the expected objects are:
-  **CONSUMER** for the contact persons associated with a company. These will act on behalf of the company. 
- **CUSTOMER** for the companies. 

##### Actions 
The following actions are expected:
- **CREATE** for creation of the objects in Magento
- **UPDATE** for updating of the objects in Magento
- **DELETE** for deletion of the objects in Magento

##### Data Structures
###### Consumer 
###### Customer

#### Products
##### Object Names
The name of the object is **PRODUCT**

##### Actions 
The supported actions are
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA0MTU0MzY2MiwtMTY5NjkyNDMwNCwtOT
EyMDgyMjcwLC03NjkzMjY0NzgsLTg2NzExNzQ5NywyMTI1OTQx
ODAyLDE0Mzc5MDM0MSwtMzk4Njc0ODk4LDEzNDkwNzU5NSwtMT
A0MTc0NDcxOF19
-->