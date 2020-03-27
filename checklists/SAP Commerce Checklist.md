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

## Architecture 
- [ ] Architectural boundaries are being respected:
	- [ ] Layer dependency is respected
	- [ ] Elements in the correct layer for Services, Facades, Controllers, DAOs, Converters, Populators and DTOs
	- [ ] Logic is in the right place for:
		- [ ]  Business Logic in Services
		- [ ]  Queries in DAOs
		- [ ] Conversions in Converters and Populators
		- [ ] UI logic and Behavior in Pages, Views, Tags, JavaScript, and CSS files
- [ ] For self-hosted projects, no changes are being done directly on standard extensions


## Extensions 
- [ ] Should be based on functionality, not a layer
- [ ] Naming conventions are respected 
- [ ] Dependencies between extensions makes sense 
- [ ] New extensions make sense  and are not superfluous
- [ ] A correct template is used in the generation of the template. This also ensures the correctness of the extension type
- [ ] Base package for the extension defined correctly
- [ ] Extension specific properties defined in the project.properties file
- [ ] No sensitive data defined in the property file

## Controllers
- [ ] Naming conventions are being respected
- [ ] No business logic should be found in controllers - use services
- [ ] No flexiblesearch queries included and no access to data layer - access only via a facade or service
- [ ] No use of models in a controller - should only be DTOs

## Data Transfer Objects 
- [ ] All data transfer objects are defined in XML file not directly as POJOs
- [ ] Should only be used in Controllers and Facades and avoided in Services and DAOs
- [ ] Naming conventions are adhered to
- [ ] All DTOs are serializable

## Converters and Populators
- [ ] Correct parent classes used 
- [ ] Naming conventions are respected
- [ ] No business logic in populators
- [ ] Prefer populators in a converter that direct logic in a converter, though it's acceptable
- [ ] No usage of models as targets. Only DTOs should be used

## Services and Facades
- [ ] All classes and methods have a single responsibility except for utility classes
- [ ] Naming conventions are adhered to 
- [ ] Encapsulation - only things that need to be public are public
- [ ] All classes and methods are reasonably sized

## Interceptors
- [ ] Naming conventions respected 
- [ ] Very minimal logic as performance impact is huge
- [ ] Correct interceptor being used e.g no saving logic in a validation interceptor

## Data Access Objects
- [ ] All queries are based on the FlexibleSearch framework
- [ ] Queries are as direct as possible - all necessary parameters must be provided
- [ ] Fetch and iterate is avoided
- [ ] Queries are parameterized and not concatenating values into the query
- [ ] Queries are only used in DAO objects 
- [ ] All DAO objects are inheriting the **de.hybris.platform.servicelayer.internal.dao.DefaultGenericDao** except where necessary to diverge
- [ ] All methods on DAO objects must not return a null result 

## Data Model 
- [ ] Defined data models are consistent with the design 
- [ ] Naming conventions are adhered to
- [ ] No collection types have been defined. Insteas, a relation should be defined
- [ ] All types are localized
- [ ] The **generate**, **autocreate**, and **extends** properties are being used correctly 
- [ ] The types on all attributes are consistent with the design and are correct
- [ ] There is no usage of the model as a DTO anywhere ( to transfer data or as a temporary mutation object)

## Dependency Management
- [ ] Use of XML-based configuration 
- [ ] No usage of component scanning using the @Component annotation except for the definition of controllers using @Controller
- [ ] No usage of JavaConfig
- [ ] Mandatory dependencies are injected via the constructor and not setters (@Required is deprecated)
- [ ] Dependencies must be injected in the configuration files or using @Resource annotation
- [ ] No usage of both @Resource and XML-based injection for the same property in a bean
- [ ] No usage of  @Autowired as it injects by type and does not work with ServiceLayerTest
- [ ] Overriding of beans is based on aliases, not bean ids wherever possible
- [ ] Copy and paste of complete bean definitions is not preferred to overriding using the parent attribute
- [ ] Properties that are not changing in value are not unnecessarily redefined when overriding
- [ ] All new bean definitions have defined aliases
- [ ] Naming conventions for bean names, ids, attributes, and aliases are adhered to

## Logging 
- [ ] A log4j or slf4j logger utility is being used or whatever the platform is using if different
- [ ] No **System.out.println()** or **e.printStacktrace()** debugging mechanisms are being used
- [ ] The correct class is being used in the log. Incorrect ones a sign of a copy and paste action
- [ ] The correct log severity is used
	
## Exception Handling 
- [ ] No catching of Exception - catch specific exception 
- [ ] No catching of checked or runtime exceptions 
- [ ] Logging of all caught exceptions 
- [ ] No ignored exceptions - catch statements with no response logic
- [ ] Use of custom exceptions where necessary 
- [ ] Logging of exception close to point of origination before re-throwing

## Resource Handling 
 - [ ] used resources must be released using 
	 - [ ] try with resources or 
	 - [ ] clean up code in the finally block

## Data Validation and Use of Null
- [ ] All input data is checked for nullity and validity
- [ ] Returning null values is avoided
- [ ] Use of utilities for checking for nullity or emptiness of response

## Session Management
- [ ] Only serializable objects are added to the session
- [ ] No models or DTOs added to session without explanation 
- [ ] No large object added to the session

## Transactions 
- [ ] Proper use of @Transaction and TransactionBody
- [ ] Use of the finally block

## Brevity 
- [ ] No use of imperative logic where declarative logic is possible - especially in looping
- [ ] No use of unnecessarily complex logic
- [ ] No code duplication
- [ ] Use of utilities

## Testing 
-  [ ]  ** Refer to the **testing** checklist. Additionally:
	-  [ ] Tests exist for all public functionality 
	-  [ ] No tests for private methods and classes
	-  [ ] The  **@UnitTest** or  **@IntegrationTest** annotation was used to specify the test type
	-  [ ] The existence of **@Test**` annotation for each test method.
	-  [ ] Integration tests have their own test data and are not using actual project data
	-  [ ] Tests contain assertions

## Documentation 
 - [ ] All public interfaces, classes, methods, and method parameters are documented
 - [ ] No acceptance of generated comments
 - [ ] No excessive use of comments - might mean naming conventions not followed
 - [ ] No documentation of the obvious - setters and getters
 - [ ] No dead code  - use SCM to remember
 
## Impex, Data Import and Export
- [ ] Clear separation between core and sample data. No sample data should exist in production
- [ ] Clear separation between essential and project data. No business data should be overridden 
- [ ] Correct placement and naming of data files
- [ ] Correct keywords in impex files are used - INSERT or INSERT_UPDATE or UPDATE or REMOVE
- [ ] It should be possible to import the same impex multiple times without causing issues
- [ ] Environment specific data has conditional logic being used for control
- [ ] Sequencing aligned with extension dependencies

## Update and Initialization 
- [ ] No update or initialization errors
- [ ] After an initialization or update, the site should be functional and no manual re-configurations should be needed as that signifies issues with data import

<!--stackedit_data:
eyJoaXN0b3J5IjpbODA3NzAzNjYzLDEzMjMyODA3NDAsLTUyMT
E1NTA5MywtMzQ3NDQwNTQxLDQ1MjEwOTgyNywtNDM0ODM0MDgy
LC0yMDcyNTczMTA1LDI2MDg0NjM0NSwtMTczMzc4NDUwNCwxMj
Y5Mzk3MDQ1LC0xMTU4MTg5NzcsODIxMDQzMTMzXX0=
-->