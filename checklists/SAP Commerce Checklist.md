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
-	[ ] Architectural boundaries are being adhered to - ensure that all code is the right place
	- [ ] Layer dependency is respected
	- [ ] All object are in their correct place - services in a services layer, facades in the facades layer, UI logic in the UI layer
	- [ ] Is logic in the right place - service logic in a service, controller logic in a controller, facade logic in a facade, queries in a DTO
- [ ] Do classes and methods have a single responsibility 
- [ ] Naming conventions are being adhered to for:
	- [ ] Extensions
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
		- [ ] UI logic and Behavior in Views or 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NTgwODU5OTksLTExNTgxODk3Nyw4Mj
EwNDMxMzNdfQ==
-->