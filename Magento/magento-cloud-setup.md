# Magento Cloud Setup 

## Purpose
This document is meant to be a guide when setting up a local instance corresponding to magento cloud. The assumption made in this document is that Magento Commerce Cloud has already has already been set up. 

## Project and Environments
The core object in a Magento Commerce is  a project. A project is made up of environments and these are usually:

 - Master - this is the production environment 
 - Staging - this is normally the testing or quality assurance environment 
 - Integration - this represent the development or initial testing environment. In some cases, this environment does not exist in name but is formed out of enviroment that are created from the staging environment

## Branching and Merging
All environments, except master, are branched/created from a parent environment. The staging environment is a branch from master and 
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTM3MDI2MjBdfQ==
-->