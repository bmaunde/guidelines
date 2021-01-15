
# Connectivity Requirements for the Consnet Connector

## Overview 
The Consnent connector is a middleware software as a service offering that integrates disparate systems such as ERP and E-Commerce systems. 

This document provides an architectural view of the ideal setup from an infrastructure perspective as well as the respective requirements and options. 

## Ideal Setup
The ideal setup is the best practice setup that is envisioned. Below is a diagramatic representation of the setup. 

![enter image description here](https://github.com/bmaunde/guidelines/blob/master/guidelines/Connectivity%20Setup.png)


## Notes on Ideal Setup
As can be inferred from the diagram above, the ideal setup requires the following: 
### Reverse Proxy 
The reverse proxy is used for the following purposes:

 - Access - providing the Consnet connector access to the backend system in a secure way 
 - SSL Termination - this role is required where the backend system might not have the capability or is not required to handle SSL communication 
 - Load Balancing - where necessary, the reverse proxy may be used to load balance between multiple backend system instances, but this is usually left to a dedicated load balancer

### SSL Infrastructure 
The ideal setup requires SSL communication between the Consnet Connector and the backend system. To achieve this, the following is required: 

 - Publicly accessible domain 
	This is a necessary because an SSL certificate can only be tied to a domain, rather than an IP address. The domain can be a new domain or a subdomain from an existing domain
	
 - SSL Certificate 
	 An SSL certificate is required to facilitate the required encryption of data as required by the HTTPS protocol. The certificate is tied to the publicly accessible domain. Where a subdomain is used, there is usually no need to acquire a new certificate.

### Firewall Rules
As the publicly accessible domain is public - so as to allow the connector to reach the backend system, there is a need to restrict access to the domain to the domain and port to the connector only. Consnet will provide the address to be whitelisted. 

### HTTP & OAuth Support 
The backend system is required to be able to authenticate to the Connetor using the OAuth protocol as that is the only supported protocol at the moment. The APIs utilised between the Connector and the backend system all utilise the HTTP protocol.

## Alternative Setup for Development
Where acceptable, such as development environments, an alternative setup can be used. 

The alternative setup involves the use of network address translation(NAT) where a publicly accessible IP address is used to point to the hidden backend system. Firewall rules would also need to be updated in this case to only allow the connector to use the nominated IP. 

Communication in this setup is not encrypted. Thus, this is not a recommended approach for Production environments
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNTUyNDg3NSwyNzc2Mjg2NDMsLTQzOT
M2Mjk3OCwtMTk0NTAxOTQ4NSwtMjA4ODc0NjYxMl19
-->