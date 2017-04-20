# Embedding in Sharepoint, Salesforce, and Mobile Apps

Much of this playbook focuses on embedding Tableau content in custom-developed web applications, but there are three common embedding scenarios that have specific requirements: Sharepoint, Salesforce, and Mobile Apps.

## Embedding in Sharepoint
Tableau provides a SharePoint Web Part to facilitate SharePoint embedding scenarios. 

The Web Part, as well as the instructions for installing it are located under `C:\Program Files\Tableau\Tableau Server\<version>\extras\embedding\sharepoint` within your Tableau Server install directory.
Using the Web Part is optional if Enable Automatic Logon option (e.g. Microsoft SSPI) is enabled for Active Directory during the Tableau Server install and configuration. In this case, simply using the built-in SharePoint Page Viewer will suffice. All you need to do is [paste the Embed Code](./01_embedding_and_jsapi.md) into the Content Editor.
The Tableau-supplied Web Part becomes necessary when using [Trusted Ticket authentication](./02_auth_and_sso.md) to achieve Single Sign On.

[Documentation for embedding in Sharepoint with Active Directory](http://onlinehelp.tableau.com/current/server/en-us/embed_ex_SP.htm)
[Documentation for embedding in Sharepoint with Local Authentication](http://onlinehelp.tableau.com/current/server/en-us/embed_ex_trustedauth.htm)

##  Embedding into Salesforce Canvas
Tableau provides a Salesforce Canvas adapter facilitate embedding scenarios free of charge. The latest version can be downloaded here: [https://www.tableau.com/sfdc-canvas-adapter](https://www.tableau.com/sfdc-canvas-adapter)

The basic requirement for embedding into SFDC is the ability to access both Tableau Server and SFDC from the same browser session using SSL. The required environment also includes Java 8, a Tomcat server version 7 or later, and OpenSSL utility to create the RSA keys.
The adapter is fully documented, and supports both SAML and Trusted Ticket authentication between SFDC and Tableau. Each SFDC user has to be mapped to a Tableau Server user identity.

## Embedding into Mobile Apps
Tableau Server and Online users can access their Tableau content via Tableau-released mobile applications, but it is often desirable to create custom mobile apps the embed Tableau content or to embed Tableau content into existing mobile apps.
To aid in that scenario, Tableau has released open-source samples called the Mobile App Bootstrap. The samples demonstrates how co connect and stay signed into a Tableau Server, embed Tableau content and interact with it using JavaScript API. They are good resources to use as a starting point for mobile application development or as samples to learn from.

The Mobile App Boostrap comes in two flavors:
* [Objective-C](https://github.com/tableau/mobile-app-bootstrap-objc) -- If your app development is done in Objective-C and/or Swift, this template will help you embed Tableau content.
* [HTML/CSS/JS/Cordova](https://cordova.apache.org/) -- [Cordova](https://cordova.apache.org/) is an engine that allows you to develop cross-device mobile applications in HTML/CSS/JS. If you are not already an ObjC/Swift shop, the Cordova template for embedding in mobile applications is a good choice.

In addition, Tableau has released the [Mobile Connected Client Plugin](https://github.com/tableau/mobile-connected-client) which can accompany the Mobile App Bootstrap, or any app that embeds Tableau content, to handle authentication so that your users do not have to continually sign in to Tableau Server.

## 

Next section: [Embedding Non-View Content](./06_embedding_non_view_content.md)

Back to [Multi-Tenancy and Row-Level Security](./04_multitenancy_and_rls.md) or the [Table of Contents](./00_table_of_contents.md)