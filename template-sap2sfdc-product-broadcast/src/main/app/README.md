
# Anypoint Template: SAP2SFDC-product-broadcast

+ [License Agreement](#licenseagreement)
+ [Use Case](#usecase)
+ [Considerations](#considerations)
	* [SAP Considerations](#sapconsiderations)
	* [Salesforce Considerations](#salesforceconsiderations)
+ [Run it!](#runit)
	* [Running on premise](#runonopremise)
	* [Running on Studio](#runonstudio)
	* [Running on Mule ESB stand alone](#runonmuleesbstandalone)
	* [Running on CloudHub](#runoncloudhub)
	* [Deploying your Anypoint Template on CloudHub](#deployingyouranypointtemplateoncloudhub)
	* [Properties to be configured (With examples)](#propertiestobeconfigured)
+ [API Calls](#apicalls)
+ [Customize It!](#customizeit)
	* [config.xml](#configxml)
	* [businessLogic.xml](#businesslogicxml)
	* [endpoints.xml](#endpointsxml)
	* [errorHandling.xml](#errorhandlingxml)


# License Agreement <a name="licenseagreement"/>
Note that using this template is subject to the conditions of this [License Agreement](AnypointTemplateLicense.pdf).
Please review the terms of the license before downloading and using this template. In short, you are allowed to use the template for free with Mule ESB Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case <a name="usecase"/>
WE NEED A NICE GUY WHO WRITE THIS FOR US!!!!

# Considerations <a name="considerations"/>

To make this Anypoint Template run, there are certain preconditions that must be considered.
All of them deal with the preparations in both source (SAP) and destination (SFDC) systems, that must be made in order for all to run smoothly.
**Failling to do so could lead to unexpected behavior of the template.**

Before continue with the use of this Anypoint Template, you may want to check out this [Documentation Page](http://www.mulesoft.org/documentation/display/current/SAP+Connector#SAPConnector-EnablingYourStudioProjectforSAP), that will teach you how to work 
with SAP and Anypoint Studio.

## Disclaimer

This Anypoint template uses a few private Maven dependencies in oder to work. If you intend to run this template with Maven support, please continue reading.

You will find that there are three dependencies in the pom.xml file that begin with the following group id: 
	**com.sap.conn.jco** 
These dependencies are private for Mulesoft and will cause you application not to build from a Maven command line. You need to replace them with "provided" scope and copy the libraries into the build path.


## SAP Considerations <a name="sapconsiderations"/>

There may be a few things that you need to know regarding SAP, in order for this template to work.

### As source of data

In order for this Anypoint Template to work, there are a few things that needs to be done in SAP first.

1. RFC destination
RFC destination of type "TCP/IP Connection" pointing to program ID on gateway needs to be created. The destination uses Unicode communication type with target system.

2. Program ID registration
RFC SDK is used to register program ID on gateway. Same program ID name is used here as in the RFC destination.

3. Partner port
Partner port needs to be defined type of Idoc of SAP release 4.x as its version. As RFC destination same RFC destination created earlier is used.

4. Partner profile
Partner profile needs to be customized type of logical system as partner type. Outbound parameter of message type MATMAS is defined in the partner profile. As receiver port an RFC destination created earlier is used. Idoc Type MATMAS01 is defined.

## Salesforce Considerations <a name="salesforceconsiderations"/>

There may be a few things that you need to know regarding Salesforce, in order for this template to work.

In order to have this template working as expected, you should be aware of your own Salesforce field configuration.

###FAQ

 - Where can I check that the field configuration for my Salesforce instance is the right one?

    [Salesforce: Checking Field Accessibility for a Particular Field][1]

- Can I modify the Field Access Settings? How?

    [Salesforce: Modifying Field Access Settings][2]


[1]: https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US
[2]: https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US


### As destination of data

This template makes use of the `External ID` field offered by Salesforce. Here is a short description on how SFDC define external ID's 

+ [What is an external ID?](http://help.salesforce.com/apex/HTViewHelpDoc?id=faq_import_general_what_is_an_external.htm)

The templates uses the External ID in order to do xRef between the entities in both systems. The idea is, once an entity is created in SFDC it's decorated with an ID from the source system which will be used afteward for the template to reference it.

You will need to create a new custom field in your **Product** entity in SFDC with the following name: 

+ `sap_external_id`

For instructions on how to create a custom field in SFDC plase check this link:

+ [Create Custom Fields](http://www.salesforce.com/smallbusinesscenter/faq/customize.jsp#customfield)



# Run it! <a name="runit"/>
Simple steps to get SAP2SFDC-product-broadcast running.


## Running on premise <a name="runonopremise"/>
In this section we detail the way you have to run you Anypoint Temple on you computer.


### Where to Download Mule Studio and Mule ESB
First thing to know if you are a newcomer to Mule is where to get the tools.

+ You can download Mule Studio from this [Location](http://www.mulesoft.com/platform/mule-studio)
+ You can download Mule ESB from this [Location](http://www.mulesoft.com/platform/soa/mule-esb-open-source-esb)


### Importing an Anypoint Template into Studio
Mule Studio offers several ways to import a project into the workspace, for instance: 

+ Anypoint Studio generated Deployable Archive (.zip)
+ Anypoint Studio Project from External Location
+ Maven-based Mule Project from pom.xml
+ Mule ESB Configuration XML from External Location

You can find a detailed description on how to do so in this [Documentation Page](http://www.mulesoft.org/documentation/display/current/Importing+and+Exporting+in+Studio).


### Running on Studio <a name="runonstudio"/>
Once you have imported you Anypoint Template into Anypoint Studio you need to follow these steps to run it:

+ Locate the properties file `mule.dev.properties`, in src/main/resources
+ Complete all the properties required as per the examples in the section [Properties to be configured](#propertiestobeconfigured)
+ Once that is done, right click on you Anypoint Template project folder 
+ Hover you mouse over `"Run as"`
+ Click on  `"Mule Application"`
In order to make this Anypoint Template run on Mule Studio there are a few extra steps that needs to be made.
Please check this Documentation Page:

+ [Enabling Your Studio Project for SAP](http://www.mulesoft.org/documentation/display/current/SAP+Connector#SAPConnector-EnablingYourStudioProjectforSAP)

### Running on Mule ESB stand alone <a name="runonmuleesbstandalone"/>
Complete all properties in one of the property files, for example in [mule.prod.properties] (../blob/master/src/main/resources/mule.prod.properties) and run your app with the corresponding environment variable to use it. To follow the example, this will be `mule.env=prod`. 


## Running on CloudHub <a name="runoncloudhub"/>
While [creating your application on CloudHub](http://www.mulesoft.org/documentation/display/current/Hello+World+on+CloudHub) (Or you can do it later as a next step), you need to go to Deployment > Advanced to set all environment variables detailed in **Properties to be configured** as well as the **mule.env**.


### Deploying your Anypoint Template on CloudHub <a name="deployingyouranypointtemplateoncloudhub"/>
Mule Studio provides you with really easy way to deploy your Template directly to CloudHub, for the specific steps to do so please check this [link](http://www.mulesoft.org/documentation/display/current/Deploying+Mule+Applications#DeployingMuleApplications-DeploytoCloudHub)


## Properties to be configured (With examples) <a name="propertiestobeconfigured"/>
In order to use this Mule Anypoint Template you need to configure properties (Credentials, configurations, etc.) either in properties file or in CloudHub as Environment Variables. Detail list with examples:
### Application configuration
**SAP Connector configuration**

+ sap.jco.ashost=your.sap.address.com
+ sap.jco.user=SAP_USER
+ sap.jco.passwd=SAP_PASS
+ sap.jco.sysnr=14
+ sap.jco.client=800
+ sap.jco.lang=EN

**SAP Endpoint configuration**

+ sap.jco.connectioncount=2
+ sap.jco.gwhost=your.sap.addres.com
+ sap.jco.gwservice=sapgw14
+ sap.jco.idoc.programid=PROGRAM_ID

**SalesForce Connector configuration**

+ sfdc.username=bob.dylan@sfdc
+ sfdc.password=DylanPassword123
+ sfdc.securityToken=avsfwCUl7apQs56Xq2AKi3X
+ sfdc.url=https://test.salesforce.com/services/Soap/u/28.0

# API Calls <a name="apicalls"/>
SalesForce imposes limits on the number of API Calls that can be made.
Therefore calculating this amount may be an important factor to
consider. Product Broadcast Template calls to the API can be
calculated using the formula:

**X / 200**

Being X the number of Products to be synchronized on each run.

The division by 200 is because, by default, Users are gathered in groups
of 200 for each Upsert API Call in the commit step. Also consider
that this calls are executed repeatedly every polling cycle.

For instance if 10 records are fetched from origin instance, then 1 api
calls to SFDC will be made ( 1).


# Customize It!<a name="customizeit"/>
This brief guide intends to give a high level idea of how this Anypoint Template is built and how you can change it according to your needs.
As mule applications are based on XML files, this page will be organized by describing all the XML that conform the Anypoint Template.
Of course more files will be found such as Test Classes and [Mule Application Files](http://www.mulesoft.org/documentation/display/current/Application+Format), but to keep it simple we will focus on the XMLs.

Here is a list of the main XML files you'll find in this application:

* [config.xml](#configxml)
* [endpoints.xml](#endpointsxml)
* [businessLogic.xml](#businesslogicxml)
* [errorHandling.xml](#errorhandlingxml)


## config.xml<a name="configxml"/>
Configuration for Connectors and [Properties Place Holders](http://www.mulesoft.org/documentation/display/current/Configuring+Properties) are set in this file. **Even you can change the configuration here, all parameters that can be modified here are in properties file, and this is the recommended place to do it so.** Of course if you want to do core changes to the logic you will probably need to modify this file.

In the visual editor they can be found on the *Global Element* tab.


## businessLogic.xml<a name="businesslogicxml"/>
Functional aspect of the Anypoint Template is implemented on this XML, directed by a batch job that will be responsible for creations/updates. The several message processors constitute four high level actions that fully implement the logic of this Anypoint Template:

1. Job execution is invoked from triggerFlow (endpoints.xml) everytime there is a new query executed asking for created/updated Contacts.
2. During the Process stage, each SFDC User will be filtered depending on, if it has an existing matching user in the SFDC Org B.
3. The last step of the Process stage will group the users and create/update them in SFDC Org B.
Finally during the On Complete stage the Anypoint Template will logoutput statistics data into the console.



## endpoints.xml<a name="endpointsxml"/>
This is file is conformed by a Flow containing the Poll that will periodically query Sales Force for updated/created Contacts that meet the defined criteria in the query. And then executing the batch job process with the query results.



## errorHandling.xml<a name="errorhandlingxml"/>
Contains a [Catch Exception Strategy](http://www.mulesoft.org/documentation/display/current/Catch+Exception+Strategy) that is only Logging the exception thrown (If so). As you imagine, this is the right place to handle how your integration will react depending on the different exceptions.



