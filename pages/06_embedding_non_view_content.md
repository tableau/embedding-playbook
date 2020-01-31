---
title: Embedding Non-View Content
---

Almost all embedded deployments of Tableau embed pre-built dashboards. But many embed other components of Tableau Server such as:

* Web Authoring screens
* The Tableau Server UI (embedded for integration and single sign-on reasons)

There are no APIs to achieve this type of embedding, but you can use iframes and any of the [single sign-on capabilities]({{ site.baseurl }}/pages/02_auth_and_sso) to achieve this.

## Embedding Web Authoring

Web Authoring is the Tableau Desktop-like functionality in the browser. The fact that Web Authoring exists allows you to pass on the self-service capabilities of Tableau to your end users. You may have pre-built dashboards for them to use, but they may have minor customization request or may want to build visualizations and dashboards from scratch.

For many situations, it is acceptable to simply allow the default behavior of allowing the user to click 'Edit' on the toolbar of an embedded dashboard. The result will be a new browser tab opened to the web authoring screen for that workbook. However, if you want to keep the user inside your application, you may prefer to embed the web authoring capabilities. To do that:

1. Each workbook has a unique url to access the web authoring screen: http://{TableauServer}/t/{site}/authoring/{wbname}/{dashboard/sheetname} Create a webpage that contains an iframe that points at the web authoring url for the workbook. You will likely make this a dynamic webpage so you do not have to hard-code this webpage for each workbook.
1. On the webpage where the embedding dashboard lives, create a link to the page that embeds the web authoring screen.
1. Make sure to hide the toolbar of the dashboard so that the user cannot click 'Edit' on the toolbar. If you need to show the toolbar, you have to get a bit tricky: Turn off editing permissions for the workbook, so that option does not appear for the user. Create a clone of the workbook that *can* be edited. The link from step 2 links to this cloned, editable workbook.

Additional Considerations:

* You will likely want to allow your users to save their edits, but not have their personalized versions affect other users. In that case, you should turn off save permissions, but allow save-as permissions. It also makes sense to give each of the users a 'sandbox' project that only they can save to.
* To give your user access to personalized content that they create, your application will have to be dynamic to show users all content they have access to, not just those you create. See the page on [Using the REST API to display dynamic content]({{ site.baseurl }}/pages/03_server_management_and_restapi)
* You can embed the web authoring screen to create a workbook from scratch instead of to edit an existing one. To do that, point the iframe at `http://{TableauServer}/t/{site}/authoringNewWorkbook/{projectId}/{datasourceName}` .
* If you are using trusted authentication, you can insert /trusted/{ticket} into the url as you would for a visualization

## Embedding other Tableau Server pages

You can also use the iframe approach to embed the Tableau Server UI. This is a rarer technique, but is useful if you want to use the out-of-the-box UI, but want to achieve single sign-on from your application.

To do so, simply embed http://{server}/trusted/{ticket}/t/{site}/ into an iframe. Be sure to enable [unrestricted tickets](http://kb.tableau.com/articles/issue/login-prompt-when-embedding-server).

<br />
*Next section: [Development and Deployment]({{ site.baseurl }}/pages/07_development_and_deployment)*
