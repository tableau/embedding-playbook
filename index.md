---
title: Tableau Embedded Analytics Playbook
layout: default
---

Embedding Tableau content allows you to add the power of interactive visualization to external applications. Common use cases of embedding are:

* Tableau dashboards as components of line-of-business or vertical applications
* Embedding into internal knowledge bases and CRM systems
* Adding interactive visualizations to blog posts
* Embedding into [custom mobile applications](https://github.com/tableau/embedding-playbook/blob/master/pages/05_embedding_in_other_apps.md#embedding-into-mobile-apps)

The act of embedding a single dashboard or visualization into a single webpage is quite simple, but a well-engineered integration requires handling other things such as authentication, authorization, content management, and performance. Depending on your integration goals, you may require the use of a variety of features and techniques.

The following list provides a summary of the key APIs and features that are used in embedded deployments. The rest of the playbook will then dive into the key embedding requirements, explain how to accomplish those requirements, and supply you with resources necessary to get up and running.

## Key features and APIs

**[Embedding API v3](https://help.tableau.com/current/api/embedding_api/en-us/index.html)** - The Embedding API v3 provides an updated developer experience and enhancements over the JavaScript API v2. The Embedding API v3 provides [web components](https://developer.mozilla.org/en-US/docs/Web/Web_Components) for embedding Tableau visualizations and is available starting with Tableau version 2021.4. When fully implemented later this year, the Embedding API v3 will support all the functionality of the JavaScript API v2, while providing future enhancements for embedding scenarios.

**[JavaScript API v2](https://help.tableau.com/current/api/js_api/en-us/JavaScriptAPI/js_api.htm)** - an API used on the front-end of the embedding application that initializes Tableau visualizations in JavaScript and provides a wealth of classes and methods driving interactivity between Tableau and your app.

If you are starting a new project, we recommend using the Embedding API v3 as it will receive feature enhancements moving forward.

**[REST API](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api.htm)** - Allows for integration between an embedding application and Tableau Server or Tableau Online by way of RESTful endpoints that manage users, content, and permissions amongst many other capabilities provided by the Tableau platform. Common use-cases include integrating user management in Tableau with user management in the embedding application, managing content and permissions according to the state of the embedding application, querying Tableau metadata so the correct information is displayed to end-users, and automating the management of Tableau Server.

**[Connected Apps](https://help.tableau.com/current/online/en-us/connected_apps.htm)** - Enables a seamless and secure authentication experience by facilitating an explicit trust relationship between Tableau Online or Tableau Server and external applications where Tableau content is embedded. The trust relationship is established and verified through an authentication token in the [JSON Web Token (JWT) standard](https://datatracker.ietf.org/doc/html/rfc7519), which uses a shared secret provided by the Tableau connected app and signed by your external application.

***NOTE:** Connected Apps is available for both Tableau Server and Tableau Online using the [Embedding API v3](https://help.tableau.com/current/api/embedding_api/en-us/index.html).*

**[External Authorization Servers (EAS)](https://help.tableau.com/current/server/en-us/connected_apps_eas.htm)** - Establishes a trust relationship between Tableau Server and the EAS. By establishing a trust relationship, you’re able to provide your users a single sign-on (SSO) experience to Tableau content embedded in your external applications through the identity provider (IdP) you’ve already configured for Tableau Server. When embedded Tableau content is loaded in your custom application, a standard OAuth flow is used. After users successfully sign in to the IdP, they are then automatically signed in to Tableau Server. To register an EAS with Tableau Server, you must have an EAS already configured. In addition, the EAS must send a valid [JSON web token (JWT)](https://datatracker.ietf.org/doc/html/rfc7519).

***NOTE:** EAS is only available for Tableau Server and the [Embedding API v3](https://help.tableau.com/current/api/embedding_api/en-us/index.html).*

**[Trusted Authentication](https://help.tableau.com/current/server/en-us/trusted_auth.htm)** - Establishes a trusted relationship between Tableau Server and an external application to provide a secure and seamless authentication experience. Unlike Connected Apps or External Authorization Server, Trusted Authentication does not follow the [JWT standard](https://datatracker.ietf.org/doc/html/rfc7519).

If you are starting a new project, we recommend using Connected Apps or External Authorization Server.

***NOTE:** Trusted Authentication is only available for Tableau Server and the [JavaScript API v2](https://help.tableau.com/current/api/js_api/en-us/JavaScriptAPI/js_api.htm).*

**[SAML, OpenID, Active Directory, Kerberos](https://help.tableau.com/current/server/en-us/security_auth.htm)** - For environments that already use one of these systems, they can be leveraged to achieve Single Sign-On (and to leverage database security in the case of Kerberos).

**[Tableau Viz Lightning web component](https://www.tableau.com/products/viz-lightning-web-component-salesforce)** - Makes it easy to [Embed Tableau views into Salesforce Lightning pages](https://help.tableau.com/current/pro/desktop/en-us/embed_ex_lwc.htm). The Tableau Viz Lightning web component is available from the Salesforce AppExchange. To embed a view, you simply drag and drop the Tableau Viz Lightning web component onto the page and then provide the URL for the Tableau view. You can embed Tableau views from Tableau Server, Tableau Online, or Tableau Public.

**Mobile App Bootstrap** - Samples/Templates for embedding in mobile apps. A great way to get up and running if you have mobile apps and want to include Tableau content or if you want a customized mobile experience for Tableau.

**[Document API](http://tableau.github.io/document-api-python/)** - API for updating data source connection information. Allows you to build content once (i.e. templates) in the scenario that you have many, structurally similar, databases for multi-tenancy or dev/test/prod scenarios.

**User Filtering** - Functionality for securely filtering a workbook based on the Tableau Server or Online login information. In multi-tenant scenarios, if you have a single database for multiple users or clients, you can use user filters to have the same content apply to the different clients, ensuring each user can only see the rows that he or she has access to.


<br />
*Next Section: [Embedding Tableau Views]({{ site.baseurl }}/pages/01_embedding_and_jsapi)*
