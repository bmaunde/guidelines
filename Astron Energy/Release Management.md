# Connect Release Management

## Purpose
This document provides guidance on how to manage the making of changes and their deployment from github to Commerce Cloud version 2 (CCv2) for the Astron Energy Connect ecommerce site. For uniformity and to avoid confusion between different people that might be involved in managing releases and deployments, it's important to adhere to the guidance provided. 

## Source Code Management
The commerce cloud platform allows for source code to be managed from any git based repository such as Github, Bitbucket, Gitlab among others. In the case of connect, Github is used. 

### Branching 
In order to manage the different stages that source code goes through from initial creation to deployment in production, multiple branches were created and are named below. 

 1. **master** - the master branch is the primary branch that is used to initiate any changes that need to be made. All users must checkout this branch to make any changes. If segregation of changes is required, feature branches can be created from the master branch and merged at a later point when the changes are ready to be integrated into the master branch. Creating feature branches for features or bug fixes is encouraged when multiple changes are being made concurrently. 
 2. **deployment** - the deployment branch is used as the development branch. This implies that all builds that need to be performed for the development environment will be done based on this branch. A pull request should be created from the master branch to the deployment branch once a release to development is required. All bug fixes for development should also be performed using a branch created of the deployment branch. 
 3. **quality_assurance** - this branch is used for all deployments to the staging environment. Builds for the staging environments should, therefore, be based on the quality_assurance branch. 
 4. **production** - the production branch is used for all releases to the production environment. Hotfixes for emergency issues in production should be performed off this branch

### Change Control
There are control mechanisms that have been put in place to manage changes so that no unwanted changes are deployed without the necessary/requisite approvals

-	When merging requests from master to deployment, reviews are required and these will allow for changes to be reviewed and approved 
-	When merging pull requests from deployment to quality_assurance, reviews and approvals are also required 
-	when merging requests from quality_assurance to production, approvals are also required

As a result of the above, no direct commits can be made onto the deployment, quality_assurance and production branches. Changes can only be integrated into these branches via pull requests. 

### Features, Bug Fixes and Hot Fixes
#### Features
When there is a new feature that needs to be added, it is advisable that a branch be created from master to cater for the new feature. The naming convention should be **feature/<short_description>**. Once the changes have been completed in the feature branch, the branch must be merged back into master via a pull request. 

#### Bug Fixes
Bug fixing branches are for bug fixes that need to be performed in the development or staging system. In order to be able to deploy the bug fix independently, a bug fix branch should be created off the corresponding branch. For development, a branch would be created off the **deployment** branch and for staging, the **quality_assurance** branch would be used.  When the changes are done, the bug fix branch should be merged into the corresponding parent branch. Once the bug fix has been deployed, the bug fix branch should then be deployed to preceding branches. A staging bug fix branch should be applied to the master and deployment branches whereas a development bug fix should be merged to the master branch. This will ensure that changes are not ovewritten upon subsequent deployments. Bug fixing branches should be named as **bugfix/<short_description>**
#### Hot Fixes
Hot fixes are emergency fixes that need to be applied to production without going through the normal change procedure of development->staging->production. A branch should be created off the **production** branch. After changes have been performed, the branch should be merged back into the production branch. After deployment, the same hotfix branch should be applied to all other branches so that when subsequent deployments are performed, the changes would not get overwritten. Hot fixing branched should be names as **bugfix/<short_descripion>**

## Release Management
To deploy any changes to any of the environments in commerce cloud, there are two actions that must be performed in the Commerce Cloud Portal.  They are as follows:

### Building 
For each environment, a build should be created before perfoming a deployment. The following conventions must be followed when creating a build. 
	
#### Versioning 
 The versioning system that has been adopted is of the following format: **\<Major>.\<Minor>.\<Patch>**
	 
 - The major version should only change with breaking changes such as after upgrade changes that impact from one major commerce version to another. 
 - The minor version should only change upon the addition of new features. 
 - The patch version should only change upon the application of bug fixes or minor changes that do not constitute a feature
 
 It should be noted that whenever the major version changes the minor and patch version numbers must be reset to zero. The same applies to the patch version number when the minor version number changes. 
 
#### Naming conventions 
Each build should be named as follows:
 - The word **Connectv** must be used as the prefix
 - All development builds must be suffixed with **-SNAPSHOT**
 - All staging build must be suffixed with **-RELEASE** 
 - All production build must have no suffix
 
		 An example would be: Connectv1.1.1-SNAPSHOT for development, Connectv1.1.1-RELEASE for staging and ConnectV1.1.1 for production

#### Source Branches
As alluded to before, the correct branch must be used for the corresponding environment as follows:

 - For development builds, the **deployment** branch must be used
 - For staging build, the **quality_assurance** branch must be used 
 - For production, the **production** branch or a tag must be used

The build version must be sequential. When a build fails, a refresh must be used to rebuild or if a new build is created, the same version number must be used so as not to change version numbers as a result of build failures. 

### Deployments
Once a build has been performed, it can be deployed to the corresponding environments. The following conventions must apply:
- All development builds (with the suffix -SNAPSHOT) must ONLY be deployed to the development environments
- All staging builds (with the suffix -RELEASE) must ONLY be deployed to the staging environments
- All production builds must ONLY be deployed to production 

#### Deployment Mode
The initialization deployment mode must be used with great care. As a result, the following conventions apply:

- The initialization deployment mode must NEVER be used when deploying to production as this results in all data being deleted
- Initialization should only be applied in development or staging when there is absolute certainty that it is the correct deployment mode to use
	 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyOTM5NTg1MDUsMTM3MjI1MTk1NiwtMT
AyMTkwNzIzNSwxNjM2NDg3NzMsLTI0MTgyOTAwNywxMjE4ODY0
MjYwLDExMTYwNDI5MDZdfQ==
-->