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
Magento works well with a linux or unix environment. That means you need to have a full-fledged unix/linux environment or a virtual machine setup on your windows system. 

You are probably using a windows based system and there are 2 options: 

 - Set-up a linux environment, preferably Ubuntu 18/20, using the Windows Subsystem for Linux. Use the following for setup: https://docs.microsoft.com/en-us/windows/wsl/install-win10
 - Set-up a Windows hypervisor based virtual machine. The following is an example guide you can use: https://geekflare.com/ubuntu-on-windows/ 
 
When working with Windows, you should be able to use IDEs or editors you are comfortable with to develop as an integration to GitHub is performed.

## Installation of Prerequisites
A number of applications and/or tools are required to run magento locally. The following is a listing of the prerequisites. 

For more info, please use the following link: https://devdocs.magento.com/guides/v2.4/install-gde/system-requirements.html
	 
#### 1. PHP
	
For Magento 2.4, please install the following:
		 
- PHP7.4
- PHP7.4-FPM
- PHP7.4-CLI ( should be there after installing PHP7.4 )
		 
You will also need to edit the php.ini files for both FPM and CLI to change the following:

- Open the CLI files in the following paths:
		 
	    /etc/php/7.4/fpm/php.ini
	    /etc/php/7.2/cli/php.ini
			    
- Change the following parameter values:
		
	    memory_limit = 2G
	    max_execution_time = 1800
	    zlib.output_compression = On	
	    date.timezone = Africa/Johannesburg
	    opcache.save_comments = 1	
	    realpath_cache_size=10M
		realpath_cache_ttl=7200
				
- Various PHP extensions are required by magento. Please verify that they all exist. 
		
	To verify, the installed extensions, use:
				
	    php -m
			    
	The following are the required extensions:
			
	    ext-bcmath
		ext-ctypt
		ext-curl
	    ext-dom
	    ext-gd
	    ext-hash
	    ext-iconv
	    ext-intl
		ext-mbstring
	    ext-openssl
	    ext-pdo_mysql
	    ext-simplexml
	    ext-soap
	    ext-xsl
	    ext-zip
	    ext-sockets
	
	If any other extensions above does not exist, please install then using apt. You should replace 'ext' with 'php'. As an example, ext-curl is php-curl. 
			
	    sudo apt-get -y install php-curl
			    	
#### 2. Composer
Install composer either using the apt or instructions on https://getcomposer.org/download/
		 

    sudo apt-get -y install composer
		 
#### 3. Nginx

	sudo apt-get -y install nginx 
		
#### 4. MariaDB 

Magento Commerce supports MariaDB. You will need to install MariaDB10.4 as follows or refer to https://devdocs.magento.com/guides/v2.4/install-gde/prereq/mysql.html for more info:
		

    sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
    sudo add-apt-repository "deb [arch=amd64,arm64,ppc64el] http://mariadb.mirror.liquidtelecom.com/repo/10.4/ubuntu $(lsb_release -cs) main"
    sudo apt update
    sudo apt -y install mariadb-server mariadb-client

   
#### 5. ElasticSearch 
   Magento 2.4 supports elasticsearch 7. To install, please follow these commands or refer to https://devdocs.magento.com/guides/v2.4/install-gde/prereq/elasticsearch.html for more detail:
   

    curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
    echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
    sudo apt update
    sudo apt install elasticsearch

#### 6. XDebug
This is an optional prerequisite but will come in handy when you need to debug issues. 
	You can install using the following command:

	sudo apt install php-xdebug
	
For more information, refer to http://xdebug.org/docs/install#linux on the section Configure PHP. 

## Configuring Prerequisites

Once you have installed the prerequisites, it is necessary to configure or set them up so they are ready to use with Magento.

#### Database Management System 
After installing MariaDB, you need to setup and create a database, user and credentials that will be used when installing Magento. 

To setup MariaDB, you will need to execute the following command: 

    sudo mysql_secure_installation
 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUyMDU1NTM2MiwtMTk5NTA5MjAwNiwtMz
M0ODk5MjgsLTc3NDU3OTc3OSwzODcxMDE2MzksLTExODc3MTA2
NjQsMTIyNjE4OTg0OSwxOTU1NTI4NzddfQ==
-->