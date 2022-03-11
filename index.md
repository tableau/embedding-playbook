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

**Embedding API v3** - The future API for embedding and programmatically interacting with individual dashboards. The Embedding API v3 provides an updated developer experience and enhancements over the JavaScript API v2. The initial support for the Embedding API v3 includes support for web components, and is available starting with Tableau 2021.4. When fully implemented later this year, the Embedding API v3 will support all the functionality of the JavaScript API v2, while providing feature enhancements. 

**JavaScript API v2** - API for embedding and programmatically interacting with individual dashboards. Useful for customizing the user experience of interacting with a dashboard and integrating with the functionality of the embedding application. We recommend that you use the Embedding API v3 if you are starting a new embedding project.

**REST API** - API for querying and managing Tableau Server or Tableau Online content and users. Useful for integrating the user management of Tableau Server with the user management of the embedding application, managing Server content and permissions based on the state of your application, querying Server metadata so the correct information is displayed to your user, and automating the management of Tableau Server amongst many other applications.

**Connected Apps and External Authorization Servers (EAS)** - The newest secure methods for authorizing embedded views. EAS and Tableau connected apps provide a way to create and manage explicit trust relationships between your Tableau Server instance, or Tableau Online site, and custom applications where Tableau content is embedded. You can control the views and projects that users of your embedded application have access to.

**Trusted Authentication/Trusted Tickets** - Method for Single Sign-On for embedded views. If users of your application sign in, when they attempt to access Tableau content, trusted authentication will allow you to pass their credentials to Tableau Server so they do not see a Tableau Server sign-in page.

**SAML, OpenID, Active Directory, Kerberos** - For environments that already use one of these systems, they can be leveraged to achieve Single Sign-On (and to leverage database security in the case of Kerberos).

**Tableau Viz Lightning web component** - An easy and flexible way to quickly bring Tableau visualizations into Salesforce. Drag and drop the Tableau Viz Lightning web component in any Salesforce page, and paste the embedded URL of any Tableau view. Uses the Tableau permissions you have set up. Embedding Tableau views into Salesforce gives you the insight of Tableau dashboards where sales people are spending their time, filtered to the correct account/opportunity/etc.

**Mobile App Bootstrap** - Samples/Templates for embedding in mobile apps. A great way to get up and running if you have mobile apps and want to include Tableau content or if you want a customized mobile experience for Tableau.

**Document API** - API for updating data source connection information. Allows you to build content once (i.e. templates) in the scenario that you have many, structurally similar, databases for multi-tenancy or dev/test/prod scenarios.

**User Filtering** - Functionality for securely filtering a workbook based on the Tableau Server or Online login information. In multi-tenant scenarios, if you have a single database for multiple users or clients, you can use user filters to have the same content apply to the different clients, ensuring each user can only see the rows that he or she has access to.


<br />
*Next Section: [Embedding Views & JavaScript API Usage]({{ site.baseurl }}/pages/01_embedding_and_jsapi)*
