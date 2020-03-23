# SAP Commerce Guidelines

  

## Purpose
The purpose of this document is to provide guidance concerning the implementation of SAP Commerce. The benefit of these guidelines and best practices will be to provide a reference framework that allows for uniformity, ease of maintenance and adherence to best practices. 

This document will be updated whenever there are new guidelines. It is best to always refer to this during the implementation of SAP Commerce projects.  This document applies to both on-premise and cloud implementations. 

## Overarching Guidelines
The standards and guidelines that follow should be considered at the onset of the project. Therefore, any required choices should be made and any posed questions must be answered

#### Architectural  Considerations
When starting a commerce project, the following considerations must be made:

- Development and Release Management
	There are 3 aspects to consider in this regard and they are:
     - Source Code Management
     Unless the customer has a tool that exists already and is compatible with expectations, git-based tools must be preferred. The first preference should be **GitHub**. 
     - Release Management and Versioning 
     Refer to the guide on Release Management for more information. There should be two permanent and protected branches - **master** & **develop**. These should be supported by temporary ***feature***, ***release***, ***bugfix*** and ***hotfix*** branches. 
     - Version System 
     A versioning system must be created and it should be in the format **major**. **minor**.**patch**. This version should be prefixed with a name that relates to the project. 
			     
		     An example is TestProjectv0.1.0

- Project Base Package
A base package that will be used across all packages should be determined. The customer can have a preference based on previous implementations. If that is not the case, the base package name should be formatted as follows:
	> \<revesed customer domain>.\<projectname> 
	
		An example would be: com.testcustomer.testproject

	It then follows that every extension that's generated should have its base package prefixed with the project base package. 
	
- Integrated Development Environment
	Without being prescriptive, any IDE should be used as long as it supports the usage of SonarLint and other tools we might use. Having said that, the IDEs of choice are **Spring Tool Suite, Eclipse and IntellijIDEA**
	SonarLint must be installed and (if not infeasible) connected to the centralized Consnet SonarQube server for rules
- Static Code Analysis
**SonarQube** is the defacto standard for static code analyses for SAP Commerce. A centralized server exists at Consnet. Therefore, a sonarqube project should be set up for any commerce project. This is mandatory as it preempts technical checks  or Cloud Readiness Checks(CRC) performed by SAP


- Continuous Integration and Deployment
Please refer to the DevOps guide for more detail. All projects should strive to have build, testing, analyses, and deployments performed as part of an automated DevOps pipeline

- Runtime & Local Development Environment 
The following applies mostly to the self-hosted / on-premise implementations.
	- DBMS: It's rare to have an un-clustered environment. All projects should, therefore, decide on a database management system to use. The choice should be predicated on the customer's preference, which is usually predicated on what they already own. If the customer has no preference, **MySQL** should be used.
	
	- Application Server:  It's not necessary to use an external application server unless the customer prefers to do so based on the fact they are well versed in a particular choice. Where such as a choice does not exist, the embedded tomcat server should suffice.
	
	- Load balancing: All projects should have a minimum of 2 storefront instances in a production environment. Usually, the backoffice node would also be a separate one. This mandates clustering. Where clustering is involved, a load balancing solution should be in place and this should perform load balancing and, if necessary, reverse proxying functions. The customer's choice takes precedence. 
	
	For the local development environments, the same choices on DBMS and Application server should be replicated to avoid surprises.
	
- Monitoring
It is important to consider monitoring tools for the runtime systems. The monitoring tool of choice is Dynatrace. For commerce cloud implementations, Dynatrace comes with the package. Otherwise, a licence is required and a determination should be made whether this can be acquired and set up. 


#### Solution Initialization
A decision on whether to bootstrap the solution from an accelerator or to create a different structure should be made based on the requirements. An accelerator is used when there is one provided by SAP and the solution to be implemented is aligned to the accelerator. B2B solutions should use the B2B Accelerator, for example. 

In the case of an accelerator being used, the  ***modulegen*** ant task and the corresponding module must be used to bootstrap the out of the box solution
		

## Implementation Guidelines

### General Architecture
SAP commerce utilizes a layered architecture that consists of the following layers:
 - UI
	 - Views, Tags, and Controllers
- Business logic
	- Facades
	- Services
 - Data
	 - Model definition
	 - Data access objects

Layer integrity must be preserved. Dependency is top-down and sequential. This means that the UI layer should not depend directly on the data layer as there is a business logic layer in-between and the data layer cannot depend on any layer as it is the bottom-most layer. 

### Extensions
Extensions are specialised modules. They must represent a set of functionality; whether business, system or infrastructure. In line with the layered architecture as describe above, the following are the expected extensions and their content:

 - UI extensions: these are typically addons and should contain only UI logic as contained in pages, views, tags, javascript, and css
 - Facades extensions: these are closely linked to the UI layer as they contain contextualized logic
 - Services extensions: these should contain business logic as well as the data layer components where the separation of the data layer is illogical
 - Data extension: this is optional as the platform extension serves part of this purpose. It is a common convention to use the services extensions to define data models and data access objects

#### Naming  Conventions
- All extension names must: 
	- Only contain alphanumeric characters
	- Only contain lowercase characters
	> Example: customordermanagementservices
	
- An addon must have the suffix ***addon***
	> Example: customb2baccelratoraddon
	
- When overriding a standard extension, the prefix ***custom***  or **\<project name>** must precede the name of the standard extension. 
	> Example: When overriding the extension sapintegrationservices, the name can be customintegrationservices or testprojectintegrationservices

- All extension names must convey intent. Therefore, 
	- An extension for services must contain the suffix ***services***
	- An extension for facades must contain the suffix ***facades***
	- An extension for data access objects must contain the suffix ***daos*** 
	
	> Examples: sapordermanagementservices, commercefacades

#### Generation 
- When generating an extension, a template extension is required. The correct template must be used and the following guidelines will help:
	- Addons 
	Addons are only created when intending to define or overwrite web content in the form of content pages, views, tags, javascript, and css among others. 
		
		The **yaddon** template must be used for all addons
	- Web Service Extension 
		Web service extensions must use the **ywebservices** (1st preference ) or **ycommercewebservices** extensions as templates
	- Other Extensions
		All other extensions must use a corresponding template if there is one, e.g., ***ybackoffice*** for backoffice extensions. Otherwise, all other extensions must use the **yempty** template

- A base package is required when  generating an extension. The name of the extension must be structured as follows:

	**<project_base_package_name>.<extension_name>**
	
	 Refer to the **Packaging** section for guidelines.

### Packaging
All package names must:
- reflect intent 
- be in **lowercase**

### Data Model
All data models or items must be created in the *-items.xml file in the resources folder of an extension. These should be in either the services/core extensions or, under special circumstances, in a dedicated data extension.

#### Items, Relations, and Enums
- For items, relations, and Enums; the names must:
	- contain ONLY alphanumeric characters
	- use **Pascal** case
		 
- Except where necessary, the name of the deployment table must be the same as the name of the item or relation.
- For item and relation attributes, **Camel** case must be utilized
	
		Example: code, deliveryDate

- For Enums, the names of the value codes must be in **Upper** case
		
		Example: SUNDAY 

- DO NOT specify the jaloclass attribute for an item type as jalo is deprecated
- DO NOT define collection types

#### Item Attributes

- Where an attribute can contain a predetermined static list of values, an Enum must be created and used to type the attribute

- Where multiple values are possible for an attribute, a relation of one-to-many relation must be created

-  Circular references - where an attribute is typed to the item it belongs to- should be avoided

- Collection types must NOT be used for any attribute. A relation of one-to-many cardinality must be created instead

- It's not possible to change the type of an attribute without raising the need to initialize the system. If not possible to initialize - such as when the solution is live - a new attribute with the correct type should be created and the old should be removed

#### Localization
Localization allows for models and model attributes to be internationalized by providing names and descriptions. These names are seen in the backoffice when a user is viewing data. This helps as it provides a meaningful context that is not provided when technical names are used. 

All models(enums and items) and their attributes must all be localized at least in English. If multiple languages are a requirement, then the relevant languages must be considered

Localizations are maintained in the **<extensions_name>-locales_<iso_code>.properties** property files in the  **resources/localization** folder of the relevant extension

### Classes and Interfaces
		
#### Interfaces 
An interface defines a contract with the outside world. It defines all the publicly accessible methods that external dependents can interact with. 

- An interface should be created for all services, facades and data access objects. The exception is only when a service is inheriting an existing implementation and not defining new publicly accessible methods
- For naming conventions, interfaces should : 
	- convey intent
	- be in **Pascal** case
	- contain ONLY alphanumeric characters
		
			Examples: AddressService, AddressDao, AddressFacade
			
- Interface method names should:
	- be in **Camel** case
	- ONLY contain alphanumeric characters
- Public interface methods must:
	-  be documented with a description for the method 
	- have documentation of the parameters and return types
	
- Interfaces must also be documented to describe the purpose of the interface

#### Implementations
Implementations are classes that define behavior for a given contract/interface. 

- The naming conventions for interfaces also apply for implementations. 
- The first implementation of an interface must be the interface name prefixed with **Default** e.g ***DefaultAddressService*** for an interface named ***AddressService***
- If inheriting from or overriding a standard implementation, the prefix **Custom** must be appended to the name of the standard implementation e.g ***CustomAddressService*** inheriting from ***DefaultAddressService***
- The prefixing with Default or Custom can be adapted to use the customer or project name if so desired but only if this is adopted for global use
- The number of lines in a class must not exceed **1000** and those in a method must not exceed **100**
- Public methods in an implementation must:
	-  be documented with a description for the method 
	- have documentation of the parameters and return types. 
- Variables should be in **Camel** case and should convey intent 
- Unless necessary, variables must be **final**
- All class-scoped variables or properties must be **private or protected** unless there is a reason not to do so
- Constants should be in **Upper** case. If word separation is desired, the  "**_**" should be used. 
- Classes must also be documented to describe the purpose of the class

### Common Object Types
The following are the guidelines for the common object types that form the three common architectural layers namely; Facades, Services, and Data Access layers. 

#### Data Access  Objects

For any item or group thereof that is created, it is common practice to create data access objects that contain the different types of queries that can be used to retrieve the objects from the database. 

 When only one item type or model is to be retrieved,  the dao must inherit from **de.hybris.platform.servicelayer.internal.dao.DefaultGenericDao** , passing the concrete model as the parameter

	Example: public class DefaultAddressDao extends DefaultGenericDao<AddressModel>

It is recommended to place all Flexible Search queries in data access objects. 

The method names should include the parameter used, or a form of uniqueness in their name (e.g: findByCode(String code) or searchUnique(…)).

A data access object should never return null for search methods. It is better to return an empty list. This will reduce the risk of null pointer exceptions.

As data access objects are interfaces and classes, the conventions for the same are applicable. Additionally, the name of a DAO interface or implementing class must contain the suffix ***Dao*** 

`Example: DefaultAddressDao`

An important performance practice to be  kept is that no search and loop should be used. As direct a query as is possible should be used to get required records. 
		
#### Data Transfer Object (DTOs)
Data transfer objects are serializable objects used to exchange data between a caller and a callee. They are normally used to pass data between a face and a controller or a controller and a web service client. DTOs contain a subset of attributes from one or more models or items. 

A common convention is that all DTOs must be declared in the *-beans.xml file of the relevant extension - commonly the facade extension. 

As DTOs are classes, the conventions defined under interfaces and implementations also apply. Additionally, the name of a DTO must contain the suffix **Data** or **Dto**

	Example: 	Interface: SaleAreasDAO
					Implementation: DefaultSalesAreasDAO

#### Services 
A service is an object that is concerned with the execution of business logic or business rules. It can utilize other services and DAOs to achieve its business goals. 

 As services are interfaces and implementing classes, the conventions for the same are applicable. Additionally, the names of service interfaces and classes must contain the suffix **Service**

			Example: 	Interface: SaleAreaDeterminationService
					Implementation: DefaultSalesAreaDeterminationService

#### Facades
A facade is a lean and focused proxy service catering for a specific use case. Whereas a service can cater for multiple use cases as a generalized implementation, a facade makes use of these services and utilizes the required functionality and flow.
 
As facades are interfaces and implementing classes, the conventions for the same are applicable. Additionally, the names of facade interfaces and classes must contain the suffix **Facade**


			Example: 	Interface: CustomerDataCompilationFacade
						Implementation: DefaultCustomerDataCompilationFacade

#### Controllers
Controllers are request handlers. They are created in the web part of an extension. 

As controllers are classes and - sometimes - interfaces, the conventions for the same are applicable. Additionally, the names of controller classes must contain the suffix **Controller** 

			Example: 	AccountPageController


#### Converters and Populators
A converter is an object that is used to convert data from one format to another. A converter may use other converters or populators. A populator is used to populate the different attributes from the a source object to a target object and it can use converters or populators to achieve its purpose.

All converters should implement the **import de.hybris.platform.servicelayer.dto.converter.Converter** interface .

All populators should implement the **de.hybris.platform.converters.Populator** interface.

As converters and populators are classes, all class conventions apply. Additionally, all converters must be suffixed with **Converter** and all populators must be suffixed with **Populator**. 
		
			Example: 	CustomerDataConverter, AddressPopulator

A common guideline is to always use a converter rather than directly using a populator when converting. All populators can be injected into a converter as they will all be called sequentially without needing to call each one individually. 

	Example:		<bean id="carouselProductConverter" parent="defaultProductConverter">
						<property name="populators">
							<list merge="true">
								<ref bean="productPricePopulator"/>
							</list>
						</property>
					</bean>

#### Interceptors 
An interceptor is used to inject logic upon a stage in the lifecycle of a model such as saving, reading, deletion. 

As interceptors are classes, all implementation class conventions apply. Additionally, all interceptors must be suffixed with **Interceptor** 

			Example: 	OrderEntryValidationInterceptor


### Dependency Management
All controllers, facades, services, converters, populators, and data access objects are declared/defined as beans. By default, all beans run as singletons unless specified. Beans can depend on other beans as all its dependencies are declared in the same extension as the bean or the extension's dependencies. 

Dependencies between extensions are declared in the **extensioninfo.xml** of an extension. 

All bean declarations are either **xml**-based definitions done in the ***-spring.xml**  or in the ***-web-spring.xml** files or as **annotation**-based definitions done in the class definition. 

Any definition mechanism can be adopted depending on needs. It is advisable to adopt a single mechanism for uniformity. Controller beans are defined almost exclusively as annotation-based beans except when overriding. This needs to be understood as the sequencing of the definitions can impact expected behavior. 

It is advisable but not mandatory to use aliases. This improves flexibility in overriding beans  as it allows overriding without complete replacement. The overridden beans can still be used, if needed, by using the id rather than the alias.

When overriding a bean, there are two approaches to use:
 - An overriding bean can be defined as a completely independent bean and use the same id or the same alias as the bean being overridden. In this case, only the generic conventions apply
 - An overriding bean can be defined inheriting from the bean that it overrides. In this case, the parent attribute should be defined and all properties defined in the parent bean should not be redefined in the overriding bean unless those properties are being adapted

It should be understood that there is a difference between a web-context specific bean and an application-context specific bean. A bean in the former cannot be depended upon by a bean in the later.

When defining bean properties or defining dependencies, ensure that you avoid circular dependencies

Dependencies are injected in 2 main ways:
- **Constructor**-based injection - this is the preferred way when xml-based definitions are commonly adopted 
- **Annotation**-based injection - autowiring can be achieved by using the @Autowired or @Resource annotations. This should be used mainly when using annotation-based bean definitions

DO NOT user property-setter based injection. The @required annotation that used to be used to mandate dependencies is deprecated.

#### Naming Conventions
When defining a bean, the following naming conventions should be followed:
- **Camel** case must be used to for bean ids, names, aliases, and properties
- ONLY use alphanumeric characters
	
		Example:	<alias  alias="productService"  name="myProductService"  />  <bean  id="myProductService"  class="com.mycompany.core.service.impl.MyProductServiceImpl"  parent="defaultProductService">  <property  name="myProductDao"  ref="myProductDao"  />  </bean>

### Essential and Sample Data Management
Almost all non-trivial solutions have some core data that all functionality depends on. This is data that is usually known at the onset of a project. Examples of this data are user roles, permissions, product categories, product catalogs and titles among many other examples.

Sample data is also important when creating a test environment

To manage this data, there are a couple of places where this data can be placed so that whenever the system is updated or initialized, this data is imported via IMPEX. The following options should be used:

- If the data is created only once and rarely changes, consider adding it in the **essentialdata-\<name>.impex** or **projectdata-\<name>.impex** file under the **resources/impex** of the relevant extension. Alternatively, the **resources/<extension_name>/import/common/** folder structure of data extensions such as the  ***initialdata** extensions
- If data is part of a content or product catalog, it should be added under the relevant files in the **resources/<extension_name>/import/coredata** folder structure 
- If data is considered as test data, then it should be created under the **resources/<extension_name>/import/coredata** folder structure if the ***initialdata** data extensions

#### Security and Sample Data
##### Security Considerations 
Several security considerations should be noted for different implementations. The following are the conventions:
- All productive environments must not contain sample users that are created as part of the out of the box solution 
- Default passwords for users that are kept as part of the solution like admin must be changed
- Out of the box clients must be removed or the default secrets for the out of the box clients should be reset 
- Usage of swagger in productive environments should be avoided
- All sensitive data, such as usernames and passwords, must not be added to project property files as they will be pushed into the git repository thereby exposing sensitive information. Instead, the properties can be declared with dummy values. They actuals must be added directly to the local.properties file for each environment for on-premise implementations or  to a static file for cloud implementations

##### Sample Data
All sample data must be removed in productive environments. There are 2 ways to achieve this. 

- The first option is to ensure that all updates do not involve sample data imports. This ensures that sample data is not imported
- The second option is to add environment based conditions to impex files so that sample data is not imported in productive environments. 
	
				#%impex.enableCodeExecution(true)
				#%impex.info("==========STARTING IMPEX IMPORT FOR URLs REGEXs==========");
				#%impex.info(Config.getParameter("license.sap.sapsystem"));

				UPDATE CMSSite;uid[unique=true];urlPatterns
				#% if: "CED".equalsIgnoreCase(Config.getParameter("license.sap.sapsystem"));
				##CONTENT
				#% endif:

The best way to manage data is to use both methods above so that when sample data is triggered for import by mistake, the conditional logic will ensure the data is not imported anyway


### Performance Considerations
Performance is  a critical part of the majority of e-commerce solutions. Below are some of the suggested things to implement to improve performance

#### Clustering 
Clustering enables load balancing and high availability. For all solutions that cater for a fairly large number of end-users, this must be implemented

#### Caching 
Caching is important to allow for improved performance. 
- Platform caching - this can be enabled by defining cache regions
- Static resource minification and caching - web resources such as css and javascript files can be minified and combined and cached using the wro4j library that is bundled with commerce
- Request caching - this can be defined for RESTful endpoints 
- Usage of CDNs - static web resources should be cached in CDNs where applicable 
- Static content - static content pages should be cached when using cloud implementations or using implementations hosted in AWS, for example, where Cloudfront can be utilized for caching purposes

## Common Design and Coding Practices

### Interface Driven Development 

All facades, services, and data access objects should define an interface and an implementation following the guidelines and practices discussed in the past sections. 

### Transactions
This cannot be stressed enough for most e-commerce solutions. The basic principle of transactional design is that every action must pass as expected and any failures must lead to a rollback. Either everything passes and data is committed or nothing passes and a rollback is initiated. 

Every transactional action must be executed in a transaction. This is implemented through the usage of the  **de.hybris.platform.tx.Transaction** utility as well as the **de.hybris.platform.tx.TransactionBody**

The example shows how to implement transactional logic. Any exception that occurs within the execute method result in a rollback of any commits that could have been performed
		
		Transaction.current().execute(new TransactionBody()
			{
				@Override

				public <T> T execute() throws Exception
				{
						//logic 
				}
			});
There are other ways to implement transactionality, but the above should be preferred. 

### Usage of Null 
Usage of Null in code or as a return value should be avoided. 

The preferred method is to throw runtime exceptions if a precondition is not satisfied or a result is not expected. The use of NULL as return value can cause NullPointerExceptions during the code execution, and also adds unnecessary null checks to the code.

Java 8 provides the Optional<> generic class which should be used in return values for methods

### Validation 

#### Parameters/Input
Parameters and input must be at least be validated for nullity. Other validations must also be performed and the correct exceptions should be thrown

Use one of the following classes to check if an input parameter was provided:
    -   **de.hybris.platform.servicelayer.util.ServicesUtil.validateParameterNotNull**
    -   **org.springframework.util.Assert** 

		Example: 	Assert.notNull(source, "Parameter source cannot be null.");

When there are validation errors, a runtime exception should be thrown

#### Output /Return Values
All results from method calls should be checked for nullity even though returning null results is not encouraged. Assumptions should not be made. 

When there are validation errors, a runtime exception should be thrown

### Exception Handling
All scenarios where an exception is expected, the exception must be explicitly handled. This means: 
- the error should be logged and necessary actions must be taken
 - empty catch statements must not be encountered in any repository
 - where necessary, custom exception classes must be utilized

Use exceptions for unusual cases that you do not expect. If you expect something to happen, you should handle it through a return value

Do not throw exceptions such as RuntimeException

Handle exceptions close to the origin code, where the issue is first seen
   
Throw exceptions up the method call stack using a custom exception relevant to that source layer. This allows you to create groups of exceptions, and handle them in a generic manner.
   
When throwing up the method call stack, always pass the original exception cause, so that you can preserve the original root cause of the exception.

 NEVER catch the “Exception” exception. The exception “RuntimeException”, and other checked exceptions, inherit from “Exception.” By catching the “Exception” exception, you are catching “RuntimeException” as well, which should be avoided. All checked exceptions should be caught and handled using appropriate catch handlers.

 Common runtime exceptions, such as referencing an out-of-bounds array element, inappropriate use of a null-pointer, and illegal cast operations, should be avoided by checking the code for such conditions.
   
An exception should be logged only once. If the same exception is logged multiple times, examining the stack-trace to try to find the original source of the exception can be difficult and confusing

### Resource Handling

Resources that are opened need to be cleaned up. Use the "finally" block to clean up open resources, or use the java 7 feature “try with resource”.

All the clean-up code should be in the finally block to close open resources.

Use “try by resource”, your resource must implement the AutoCloseable interface.

### Logging 
Logging should be performed at the correct logging level. Logging at the INFO level should be minimally done and should not be used for debugging purposes. The DEBUG level should be used for that. When handling exceptions, the WARN or ERROR level should be used. 

The **org.apache.log4j.Logger** logger or **org.slf4j.Logger** logger must be used  universally. If the platform is using a different logger, then that can be used

The logging severities must be used correctly as follows: 
- ERROR - where an actual error has been encountered that results in the stopping of a process or system
- DEBUG - where diagnostic information is useful 
- INFO - where useful information like stages in a process- not diagnostic information -  is necessary
- WARN - where something that needs to be noted and is not merely informatonal is necessary e.g	 where a 


### Automated Testing
Please refer to the guide on testing for more information. 
Testing is a critical part of any non-trivial solution. Commerce solutions are by no means trivial. 
As common conventions for commerce, the following are expected as minimums:
- All services, facades, data access object, and converters/populators must have unit tests against them
- For a full repository, a minimum of 80% test coverage is expected
- When writing tests, take note that the Junit tenant will be utilized to execute the tests
- Unit tests must be unit tests and not integration tests. That is to say that all dependencies must be mocked rather than having the actual objects injected
- Without being prescriptive, Test Driven Development should be practiced as that makes development faster and self-verifiable

### Brevity 
For readability and conciseness, the following is encouraged:

-	Use declarative programming wherever possible. This advocates the usage of streams and lambdas, as an example
-	Use utilities for checking data validity such as:
	-	CollectionsUtils
	-	StringUtils
- Avoid overuse of inline comments. It is usually a symptom that naming is poor or imprecise logic is being used when there is an overuse of comments

### Documentation
Each class and public method (except Getter and Setter methods) should have a Javadoc. This is especially the case for public methods representing an API for other extensions. Note that these Javadoc’s should still be accompanied by descriptive methods and parameters.
-   Avoid using automatically ­generated comments.
-   Do not document property style Getters and Setters (e.g: on a DTO).
-   Use Javadoc to document what a method is doing, instead of using inline comments.

### Libraries 
Libraries must be used with care and a review should be performed with the team or technical leads to ensure that vulnerable libraries are not used and also to ensure that libraries are not duplicated in multiple extensions.

<!--stackedit_data:
eyJoaXN0b3J5IjpbNTIxMTcxMTkwLC02NTgxOTEzNTRdfQ==
-->