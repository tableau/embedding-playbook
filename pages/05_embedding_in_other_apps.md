---
title: Embedding in SharePoint, Salesforce, and Mobile Apps
---

Much of this playbook focuses on embedding Tableau content in custom-developed web applications, but there are three common embedding scenarios that have specific requirements: SharePoint, Salesforce, and Mobile Apps.

## Embedding in SharePoint

Tableau provides a SharePoint Web Part to facilitate SharePoint embedding scenarios.

The Web Part, as well as the instructions for installing it are located under `C:\Program Files\Tableau\Tableau Server\<version>\extras\embedding\sharepoint` within your Tableau Server install directory.
Using the Web Part is optional if Enable Automatic Logon option (e.g. Microsoft SSPI) is enabled for Active Directory during the Tableau Server install and configuration. In this case, simply using the built-in SharePoint Page Viewer will suffice. All you need to do is [paste the Embed Code]({{ site.baseurl }}/pages/01_embedding_and_jsapi) into the Content Editor.
The Tableau-supplied Web Part becomes necessary when using [Trusted Ticket authentication]({{ site.baseurl }}/pages/02_auth_and_sso) to achieve Single Sign-On.

You can embed Tableau Online views into SharePoint as well, but neither Active Directory nor Trusted Tickets are supported for Online.

**See also**

* [Documentation for embedding in SharePoint with Active Directory](https://onlinehelp.tableau.com/current/pro/desktop/en-us/help.htm#embed_ex_SP.html)
* [Documentation for embedding in SharePoint with Local Authentication](https://onlinehelp.tableau.com/current/pro/desktop/en-us/help.htm#embed_ex_trustedauth.html)

## Embedding into Salesforce Lightning

Tableau provides a free Salesforce Lightning web component that facilitates embedding scenarios. The latest version of the Tableau Viz Lightning web component is available from the [AppExchange](https://appexchange.salesforce.com/appxListingDetail?listingId=a0N4V00000GF1cSUAT)

The Tableau Viz Lightning web component makes it easy to embed Tableau views in Salesforce Lightning pages. To install the Tableau Viz Lightning web component in your Salesforce org you just need to be a Salesforce Administrator. After it’s installed, you can just drag and drop the Tableau component on any Salesforce page and set the URL to any view on Tableau Server, Tableau Online, or Tableau Public.

The Tableau Viz Lightning web component lets you quickly filter views based on the Lightning page you embed them in. You can also select filters based upon Tableau and Salesforce fields.

Based on the Salesforce Lightning Web Component framework and the Tableau JavaScript API for embedding, developers can extend and customize the open-source Tableau Viz Lightning web component project on GitHub. For more information, see [Tableau Viz Lightning Web Component](https://tableau.github.io/tableau-viz-lwc) and the [Tableau Viz LWC Samples](https://github.com/tableau/tableau-viz-lwc-samples).

The Tableau Viz Lightning web component does not support Salesforce Classic. For information about using the Salesforce Canvas adapter, see the following Knowledge Base article, [Tableau Viz Lightning Web Component Does Not Work On Salesforce Classic](https://kb.tableau.com/articles/issue/tableau-viz-lightning-web-component-does-not-work-on-salesforce-classic).

For convenience, you can configure Tableau to use Single Sign-On (SSO) with Salesforce. The Tableau Viz Lightning web component only supports SAML as the SSO method. The SAML IdP used for Tableau authentication must be either the Salesforce IdP or the same IdP that is used for your Salesforce instance. Configuring Tableau Server or Tableau Online requires Tableau administrator permissions.

* For information about setting up SSO with Tableau Online, see [Configure SAML with Salesforce](https://help.tableau.com/current/online/en-us/saml_config_salesforce.htm) and [Configure SAML for Tableau Viz Lightning Web Component](https://help.tableau.com/current/online/en-us/saml_config_TOL_LWC.htm).

* For information about setting up SSO with Tableau Server, see [Configure SAML for Tableau Viz Lightning Web Component](https://help.tableau.com/current/server/en-us/saml_config_LWC.htm).


## Embedding into Mobile Apps

Tableau Server and Online users can access their Tableau content via Tableau-released mobile applications, but it is often desirable to create custom mobile apps the embed Tableau content or to embed Tableau content into existing mobile apps.
To aid in that scenario, Tableau has released an open-source sample called the Mobile App Bootstrap. The samples demonstrates how co connect and stay signed into a Tableau Server, embed Tableau content and interact with it using JavaScript API. They are good resources to use as a starting point for mobile application development or as samples to learn from.

The Mobile App Bootstrap comes in two flavors:

* [Objective-C](https://github.com/tableau/mobile-app-bootstrap-objc) -- If your app development is done in Objective-C and/or Swift, this template will help you embed Tableau content.
* [HTML/CSS/JS/Cordova](https://cordova.apache.org/) -- [Cordova](https://cordova.apache.org/) is an engine that allows you to develop cross-device mobile applications in HTML/CSS/JS. If you are not already an ObjC/Swift shop, the Cordova template for embedding in mobile applications is a good choice.

In addition, Tableau has released the [Mobile Connected Client Plugin](https://github.com/tableau/mobile-connected-client) which can accompany the Mobile App Bootstrap, or any app that embeds Tableau content, to handle authentication so that your users do not have to continually sign in to Tableau Server.

<br />
*Next section: [Embedding Non-View Content]({{ site.baseurl }}/pages/06_embedding_non_view_content)*
