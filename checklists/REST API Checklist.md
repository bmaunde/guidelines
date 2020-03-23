
# REST API Checklist

## Purpose 
This document is meant to be used as part of a code review for the pull requests that are initiated as part of Continous Integration on projects. The checklist helps to ensure that project readiness and code quality are always at the forefront.

It is important to ensure that all the relevant checklists are satisfied before integrating code or configuration into the develop branch. 

## Endpoints
- [ ] All endpoints are using nouns relating to the resources being interacted with
- [ ] All the nouns are plural
- [ ] Unique keys are used as part of the URL for all endpoints relating to an individual resource
- [ ] Relations between parent and child arent reflected in the endpoint flow

## Actions 
- [ ] HTTP Methods are used correctly as per the guideline 

## Headers
 - [ ] Authentication is used properly using headers and not a username and password as part of URL
 - [ ] Serialization formats are explicitly catered for
 - [ ] Where caching is expected, headers are utilized

## Response
- [ ] Status codes are used and used correcly to indicate success, failure or error
- [ ] A response payload is defined for all instances where a response is required (in GET actions and when errors have occured)
- [ ] Payload formats are defined correctly

## Searching, Filtering, Sorting, and Paging

### Searching 
- [ ] A parameter is defined to contain the query 
- 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3MDg4NTg2OF19
-->