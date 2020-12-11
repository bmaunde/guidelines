# Magento Cloud Setup 

## Purpose
This document is meant to be a guide when setting up a local instance corresponding to magento cloud. The assumption made in this document is that Magento Commerce Cloud has already has already been set up. 

## Project and Environments
The core object in a Magento Commerce is  a project. A project is made up of environments and these are usually:

 - Master - this is the production environment 
 - Staging - this is normally the testing or quality assurance environment 
 - Integration - this represent the development or initial testing environment. In some cases, this environment does not exist in name but is formed out of enviroment that are created from the staging environment

## Branching and Merging
All environments, except master, are branched/created from a parent environment. The staging environment is a branch from master and any integration is supposed to branch from the staging environment. A branch operation can be triggered to create a child environment. An environment from which another environment branches is called a parent environment. 

Depending on whether continuous deployment is set up, deployments or merging happens in the reverse direction of branching. To deploy to the parent environment, a merge operation can be performed from the child environment.  There are other ways to deploy such as to push the changes directly to the target environment. Where continuous deployments are set up, pull request merges in the git based environment triggers a deployment.

## Interaction with Magento Cloud
There are two ways to interact with Magento Cloud. These are:

 - The Web Interface 
 - The Magento CLI
It is advisable to get comfortable using the CLI tool. You can pretty do everything you can do with the Web interface and more.

## Prerequisites 
Before you start setting up your local environment you will need the following:

 - An account on magento.com. if you do not have one, you can register here https://account.magento.com/customer/account/create/ 
 - An account on the Magento Cloud project. If this is not available, please request for one from the project owner or team lead


## Development System 
The magento development works with a linux or unix environment. That means you need to have a full-fledged unix/linux environment or a virtual machine setup on your windows system. 

You are probably using a windows based system and there are 2 options: 

 - Set-up a linux environment, preferably Ubuntu 18/20, using the Windows Subsystem for Linux. Use the following for setup: https://docs.microsoft.com/en-us/windows/wsl/install-win10
 - Set-up a Windows hypervisor based virtual machine. The following is an example guide you can use: https://geekflare.com/ubuntu-on-windows/ 
 
When working with Windows, you should be able to use IDEs or editors you are comfortable with to edit the source
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzMTEwMjc0NF19
-->