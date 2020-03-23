# SAP Commerce Code Review Checklist

## Purpose
This document is meant to be used as part of a code review for the pull requests that are initiated as part of Continous Integration on projects. Another dimension of coverage is the initial considerations that are made on a project. The checklist helps to ensure that project readiness and code quality are always at the forefront.

It is important to ensure that all the relevant checklists are satisfied before integrating code or configuration into the develop branch. 

## Overarching Considerations Checklist
- [ ] SCM tool has been chosen and all team members have access
- [ ] Release management activities have been performed (refer to the release management checklist)
- [ ] A base package for the project has been defined
- [ ] A SonarQube project has been created 
- [ ] All team members have SonarLint installed on their IDEs
- [ ] All infrastructure relate choices have been made
	- [ ]  Integration systems
	- [ ] Database
	- [ ] Application Server
	- [ ] Load Balancing 
	- [ ] Monitoring 
- [ ] Domain registration and SSL certificates acquisition processes have been initiated


## Code Review (CI) Checklist
## General
- [ ] Do classes and methods have a single responsibility 
- [ ] Naming conventions are being adhered to for:
	- [ ] Packages
	- [ ] Interfaces and Classes
	- [ ] Methods
	- [ ] Variables, Properties and Constants
- [ ] Encapsulation - only things that need to be public are public
- [ ]  All services, facades are created following the interface driven development
- [ ] All classes and methods a reasonably sized
- [ ] All tests are provided and no tests for private methods
- [ ] All public elements are documented - interfaces, classes, methods, method parameters
- [ ] Use of Null has been restricted 
- [ ] There is no dead code (commented out code)
- [ ] Logging is performed via loggers and not **Sytem.out.println()** and **e.printStackTrace()**

## Architecture 
- [ ] Architectural boundaries are being respected:
	- [ ] Layer dependency is respected 
	- [ ] Elements in the correct layer for Services, Facades, Controllers, DAOs, Converters, Populators and DTOs
	- [ ] Logic is in the right place for:
		- [ ]  Business Logic in Services
		- [ ]  Queries in DAOs
		- [ ] Conversions in Converters and Populators
		- [ ] UI logic and Behavior in Pages, Views,Tags, JavaScript and CSS files
- [ ] DTOs are defined in beans.xml files and not directly as pojos
- [ ] For self-hosted projects, no changes are being done directly on standard extensions

## Extensions 
- [ ] Naming conventions are respected 
- [ ] Dependencies between extensions makes sense 
- [ ] New extensions make sense  and are not superfluous
- [ ] A correct temnplate is used in the generation of the template. This also ensures correctness of the extension type
- [ ] Base package for extension defined correctly
- [ ] Extension specific properties defined in the project.properties file
- [ ] No sensitive data defined in the property file

## Data Model 
- [ ] Defined data models are consistent with design 
- [ ] Naming conventions for all types -  Items, Relations, Enums, and Atrributes are being following 
- [ ] No collection types have been defined. Insteas relation should be defined
- [ ] All types are localized
- [ ] The **generate** and **autocreate** and **extends** properties are being used correctly 
- [ ] The types on all attributes is consistent with design and is correct

## Queries 
- [ ] All queries are based on the FlexibleSearch framework
- [ ] Queries are as direct as possible - all necessary parameters must be provided
- [ ] Fetch and iterate is avoided
- [ ] Queries are parameterized and not concatenating values into the query
- [ ] Queries are only used in DAO objects 
- [ ] All DAO objects are inheriting the **de.hybris.platform.servicelayer.internal.dao.DefaultGenericDao** except where necessary to diverge
- [ ] All methods on DAO objects do not return a null result 

## Dependency Management
- [ ] All dependencies should be defined uniformly as per project convention - either all as spring-xml definitions or annotations
- [ ] Overriding of beans is based on aliases not bean ids whereever possible
- [ ] Copy and paste of complete bean definitions is not prefered to overriding using the parent attribute
- [ ] Properties that are not changing in value are not unnecessarily redefined when overriding
- [ ] All new bean definitions have defined aliases
- [ ] Dependencies are injected via the constructor and not setters (@Required is deprecated)
- [ ] Naming conventions for bean names, ids, attributes, and aliases are adhered to

## Data Import and Export
- [ ] Clear separation between core and sample data. No sample data should exist in production
- [ ] Clear separation between essential and project data. No business data should be overridden 
- [ ] Correct placement and naming of data files
- [ ] Correct keywords in impex files are used - INSERT or INSERT_UPDATE or UPDATE or REMOVE 
- [ ] Environment specific data has conditional logic being used for control

## Update and Initialization 
- [ ] No update or initialization errors
- [ ] After an initialization or update, the site should be functional and no manual re-configurations should be needed as that signifies issues with data import
- [ ] 


## Logging 
- [ ] A log4j or Sl4j logger utility is being used
- [ ] No **System.out.println()** or **e.printStacktrace()** debugging mechanisms are being used
- [ ] The correct class is being used in the log. Incorrect ones a sign of a copy and paste action
- [ ] The correct log severity is being used as follows:
	- [ ] ERROR - where an actual error has been encountered
	- [ ] DEBUG - where debug information is useful 
	- [ ] INFO - only where useful information and not debug information is necessary
	- [ ] WARN - where something that needs to be noted and is not merely informatonal is necessary
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTEyMjgyMTU2NiwyNjA4NDYzNDUsLTE3Mz
M3ODQ1MDQsMTI2OTM5NzA0NSwtMTE1ODE4OTc3LDgyMTA0MzEz
M119
-->