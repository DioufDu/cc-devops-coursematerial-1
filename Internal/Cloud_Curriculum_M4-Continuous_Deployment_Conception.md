# Cloud Curriculum - M4 - Continous Deployment Conception

## Ruby Automation scripts
https://github.wdf.sap.corp/agile-se/cc-m4-automation-scripts

## Applications
* [cc-bulletinboard-ads-spring-jersey](https://github.wdf.sap.corp/agile-se/cc-bulletinboard-ads-spring-jersey)
	* Dependencies: PostgreSQL
* [cc-bulletinboard-statistics](https://github.wdf.sap.corp/agile-se/cc-bulletinboard-statistics)
	* Dependencies: Currently none

## Jenkins System

* Jenkins_03 UI: http://mo-f8268fdb9.mo.sap.corp:8080/jenkins/
* Monsoon Machine Details: https://monsoon.mo.sap.corp/organizations/agiledevelopmentenabling_ci_cd/projects/jenkins_trainings_2015/instances/mo-f8268fdb9

## Used Cloud Foundry environments

Cloud Foundry Endpoint: https://api.cf.neo.ondemand.com

All CF Stages will be reflected in the AgileSE organization:

    cf target -o AgileSE
    cf spaces

### Used Spaces
* Integration
	* [cc-bulletinboard-ads-spring-jersey](cc-bulletinboard-ads-spring-jersey-integration.cfapps.neo.ondemand.com)
* Acceptance
	* [cc-bulletinboard-ads-spring-jersey](cc-bulletinboard-ads-spring-jersey-acceptance.cfapps.neo.ondemand.com)
* Production
	* TODO

## Prerequisites

### Software
* Jenkins server
* Jenkins Ruby plugin (see link below)
* Jenkins Cloud Foundry plugin (see link below)
* Jenkins Credentials Binding plugin (see link below)
* Jenkins SCM Sync Plugin (see link below)
* cf CLI installed on the Jenkins server (see link below)

### Technical Github User Creation
TODO: Write desription here

## cf CLI installation on jenkins (RedHat system)
wget <the current redhat package url (see below)> -O cf-cli.rpm
sudo rpm -i cf-cli.rpm

## Stages

* Commit Stage : Executed within Jenkins (Build + Unit Testing)
* Integration Deployment : Deploy to Cloud Foundry integration space
* Integration Stage : Execute integration test-suite
* Acceptance Deployment : Deploy to Cloud Foundry acceptance space
* Acceptance Stage : Execute acceptance test-suite
* Production Deployment : Deploy to Cloud Foundry production space using a blue-green deployment

## Technical CF User Creation Workflow
* Create User at [Hana Platform](https://account.hana.ondemand.com/)
* [Self-service Cloud Foundry registration](http://onboard.cfapps.neo.ondemand.com/)
* Open Ticket to assign user to an existing organization and it's spaces as SpaceDeveloper [here](https://jtrack.wdf.sap.corp/secure/CreateIssue!default.jspa)
* Alternative: The org/space manager assigns the roles as needed, but there's still a bug caused by the cf cli

## Scripting

### Available Shell Scripting Variables in Jenkins

The following variables are available to shell scripts:

* BUILD_NUMBER
	* The current build number, e.g. "153"
* BUILD_ID
	*	The current Build-ID, e.g. "2005-08-22_23-59-59" (YYYY-MM-DD_hh-mm-ss)
* BUILD_DISPLAY_NAME
	* Display name of the current build, e.g. "#153"
* JOB_NAME
	* Build projectname , e.g. "foo" or "foo/bar"
* BUILD_TAG
	* "jenkins-${JOB_NAME}-${BUILD_NUMBER}"
* EXECUTOR_NUMBER
	* The running number of the build process executing the current build
* NODE_NAME
	* Name of Build-Slaves, when build on a slave, or "master" when build on a master
* NODE_LABELS
	* Whitespace separated list of labels assigned to the node
* WORKSPACE
	* Absolute Workspace path
* JENKINS_HOME
	* Absolute path where the master stores it's data
* JENKINS_URL
	* Absolute url of the jenkins instance, e.g. http://server:port/jenkins/
* BUILD_URL
	* Absolute url of the build, e.g. http://server:port/jenkins/job/foo/15/
* JOB_URL
	* Absolute url of the job, e.g. http://server:port/jenkins/job/foo/
* SVN_REVISION
	* Subversion revisionnumber of the version checked out in the job, e.g. "12345"
* SVN_URL
	* SVN url of the checked out version

## Links

### Used software
* [Jenkins Cloud Foundry Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Cloud+Foundry+Plugin)
* [Jenkins Ruby Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Ruby+Plugin)
* [Jenkins SCM Sync Plugin](https://wiki.jenkins-ci.org/display/JENKINS/SCM+Sync+configuration+plugin)
* [Jenkins Credentials Binding Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Credentials+Binding+Plugin)
* [CF CLI Releases](https://github.com/cloudfoundry/cli/releases)

### Additional documentation
* [Building pipelines by linking Jenkins jobs](http://blog.akquinet.de/2011/11/09/building-pipelines-by-linking-jenkins-jobs/)
* [SAP Cloud Foundry Delivery process documentation](https://github.wdf.sap.corp/cloudfoundry/cf-docs/wiki/Delivery-Process)
* [IoT_Siemens: CI-Process](https://wiki.wdf.sap.corp/wiki/display/IoTDev/Development+Landscape#DevelopmentLandscape-CIProcess)
* [TwoGO WIKI] (https://github.wdf.sap.corp/TwoGo/TwoGo/wiki)
* [TwoGO Jenkins Cookbook](https://github.wdf.sap.corp/TwoGo/TwoGo-Jenkins-Cookbook)
