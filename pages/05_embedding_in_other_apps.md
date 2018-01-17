---
title: Embedding in Sharepoint, Salesforce, and Mobile Apps
---

Much of this playbook focuses on embedding Tableau content in custom-developed web applications, but there are three common embedding scenarios that have specific requirements: Sharepoint, Salesforce, and Mobile Apps.

## Embedding in Sharepoint

Tableau provides a SharePoint Web Part to facilitate SharePoint embedding scenarios.

The Web Part, as well as the instructions for installing it are located under `C:\Program Files\Tableau\Tableau Server\<version>\extras\embedding\sharepoint` within your Tableau Server install directory.
Using the Web Part is optional if Enable Automatic Logon option (e.g. Microsoft SSPI) is enabled for Active Directory during the Tableau Server install and configuration. In this case, simply using the built-in SharePoint Page Viewer will suffice. All you need to do is [paste the Embed Code]({{ site.baseurl }}/pages/01_embedding_and_jsapi) into the Content Editor.
The Tableau-supplied Web Part becomes necessary when using [Trusted Ticket authentication]({{ site.baseurl }}/pages/02_auth_and_sso) to achieve Single Sign-On.

You can embed Tableau Online views into Sharepoint as well, but neither Active Directory nor Trusted Tickets are supported for Online.

**See also**

* [Documentation for embedding in Sharepoint with Active Directory](https://onlinehelp.tableau.com/current/pro/desktop/en-us/help.htm#embed_ex_SP.html)
* [Documentation for embedding in Sharepoint with Local Authentication](https://onlinehelp.tableau.com/current/pro/desktop/en-us/help.htm#embed_ex_trustedauth.html)

##  Embedding into Salesforce Canvas

Tableau provides a Salesforce Canvas adapter facilitate embedding scenarios free of charge. The latest version can be downloaded here: [https://www.tableau.com/sfdc-canvas-adapter](https://www.tableau.com/sfdc-canvas-adapter)

The basic requirement for embedding into Salesforce is the ability to access both Tableau Server and Salesforce from the same browser session using SSL. The required environment also includes Java 8, a Tomcat server version 7 or later, and OpenSSL utility to create the RSA keys.
The adapter is fully documented, and supports both SAML and Trusted Ticket authentication between Salesforce and Tableau. Each Salesforce user has to be mapped to a Tableau Server user identity.

## Embedding into Mobile Apps

Tableau Server and Online users can access their Tableau content via Tableau-released mobile applications, but it is often desirable to create custom mobile apps the embed Tableau content or to embed Tableau content into existing mobile apps.
To aid in that scenario, Tableau has released an open-source sample called the Mobile App Bootstrap. The samples demonstrates how co connect and stay signed into a Tableau Server, embed Tableau content and interact with it using JavaScript API. They are good resources to use as a starting point for mobile application development or as samples to learn from.

The Mobile App Boostrap comes in two flavors:

* [Objective-C](https://github.com/tableau/mobile-app-bootstrap-objc) -- If your app development is done in Objective-C and/or Swift, this template will help you embed Tableau content.
* [HTML/CSS/JS/Cordova](https://cordova.apache.org/) -- [Cordova](https://cordova.apache.org/) is an engine that allows you to develop cross-device mobile applications in HTML/CSS/JS. If you are not already an ObjC/Swift shop, the Cordova template for embedding in mobile applications is a good choice.

In addition, Tableau has released the [Mobile Connected Client Plugin](https://github.com/tableau/mobile-connected-client) which can accompany the Mobile App Bootstrap, or any app that embeds Tableau content, to handle authentication so that your users do not have to continually sign in to Tableau Server.

<br />
*Next section: [Embedding Non-View Content]({{ site.baseurl }}/pages/06_embedding_non_view_content)*
