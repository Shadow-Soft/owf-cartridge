#Ozone Widget Cartridge on OpenShift


This git repository contains the source for the owf-widget-cartridge RPM package.

**Dependencies:**
* JBoss EWS Cartridge (openshift-origin-cartridge-jbossews)
* PostgreSQL Cartridge (openshift-origin-cartridge-postgres)

**Provides:**
 * Ozone Widget Framework

##Building the RPM package

Prerequisites

* RHEL, CentOS, or Fedora with the "Development Tools" group installed

* The Tito rpm build tools

        > yum install tito

* Git (if not already installed)

        > yum install git

* Clone & build the repo

        > git clone https://github.com/Shadow-Soft/owf-widget-cartridge.git

        > cd owf-widget-cartridge

        > tito init (first build after cloning only)

        > tito tag (for first build after cloning, only needed when you want to create a new tag...e.g. 'release')

        > tito build --rpm --test

Tito will generate the rpm package in /tmp/tito/noarch/owf-cartridge{*}.rpm

##Deploying the Cartridge
The cartridge can be deployed as either an RPM installed on the OpenShift Enterprise (on-premisis) PaaS, or deployed as a custom cartridge in environments such as OpenShift Online.

###OpenShift Enterprise
Install the RPM using your favorite package manager

        > yum localinstall /path/to/owf-cartridge-{*}.rpm
        
Use the RedHat Cloud (rhc) command line utility to create a new Ozone application

        > rhc app create ozone shadowsoft-owf-7.0

###OpenShift Online
using the RedHat Cloud (rhc) command line client utility

        > rhc create-app ozone https://raw.github.com/Shadow-Soft/owf-widget-cartridge/master/metadata/manifest.yml
        
using the OpenShift Online management console
* Log into your OpenShift Online account at https://openshift.redhat.com
* Navigate to "MyApps" then to "Applications"
* Select "Add Application" (or create first application if you don't have any yet)
* Scroll down to the "Code Anything" section and enter the manifest URL in the "URL to a cartridge definition"
  https://raw.github.com/Shadow-Soft/owf-widget-cartridge/master/metadata/manifest.yml

##Using the Cartridge
Once deployed, the source repository (Git repo) provided by the create-app command contains all of the Ozone Widget Framework configuration information under /path/to/repo/.openshift/configuration/owf directory
* OWFsecurityContext.xml - defines the Spring Security configuration for the app.  Default is Basic Spring Security
    * Default users & passwords are *jimi/password*, *bob/password*, *chris/password*, *laura/password*
* security/ - has a few alternative Spring Security configurations
* ozone-security-beans/ - provide Spring Bean files pre-wired for the alternative Spring security configurations
* themes/ - a place to put customized Ozone themes


##Known Issues
* This issue applies if deploying on OpenShift Origin (community version) on any non-RHEL system (e.g. CentOS or Fedora).  The OWF Cartridge depends on the JBossEWS cartridge (openshift-origin-cartridge-jbossews), which in turn depends on the tomcat7 package.  However, the "tomcat7" package only exists in RedHat repositories and is only accessible if you have a RedHat entitlement.  The community equivalent of "tomcat7" is the "tomcat" (ommit 7) package.  However that package does not satisfy JBossEWS cartridge dependency.
