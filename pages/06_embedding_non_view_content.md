---
title: Embedding Non-View Content
---

Almost all embedded deployments of Tableau embed pre-built dashboards. But some embed other components of Tableau such as:

* web authoring experience
* The Tableau Server UI (embedded for integration and single sign-on reasons). (Note that Tableau does not offer support for this deployment).

The easiest and best way to include web authoring in your embedding scenario is to use the [Tableau Embedding API v3](https://help.tableau.com/current/api/embedding_api/en-us/index.html). The Tableau Embedding API v3 provides a web component, `<tableau-viz-authoring>`, which in just a few lines of HTML code, allows users to author the view in your web application (if permissions allow). You can also use the Embedding API v3 to call JavaScript methods that let you to customize the authoring lifecycle. For example, you can dynamically configure and switch between the authoring or viewing experience.

## Embedding Web Authoring

Web authoring allows someone to edit a visualization just as they would in Tableau Desktop.
Embedding a view with web authoring gives your users the ability to modify and update a view, all while staying in their workflow. Embedded web authoring means that you can create comprehensive embedded applications for your customers that include options for self-servicing.

When it comes to editing a visualization, it is often acceptable to simply use the default settings and allow the user to click **Edit** on the toolbar of an embedded dashboard. The result will be a new browser tab opened to the web authoring screen for that workbook. However, now you can embed the Tableau authoring experience directly in the application. There's no need to have users jump to another window or tab. You can use the **Edit** button in the toolbar, or create your own edit button that matches the look and feel of your application. To do that:

* Add a `<tableau-authoring-viz>` element to your HTML code or use the Embedding API v3 JavaScript methods to create a `TableauAuthoringViz` object. Set the `src` attribute or property to the URL of the viz. Then link to, or import, the Embedding API v3 library. 

   ```html
   <tableau-authoring-viz id="AuthoringViz"       
     src='https://my-server/authoring/my-workbook/my-view'>
    </tableau-authoring-viz>

   ```

### Create a custom editing workflow

To create your own editing workflows, you can hide the default **Edit**, **Close**, and **Edit in Desktop** buttons on the toolbar, by setting attributes on the `<tableau-authoring-viz>` and `<tableau-viz>` web components, or by setting those properties on the JavaScript objects. And then create your own custom buttons in your application and switch between authoring and viewing.

You could also change a setting to suppress the default actions that occur when the **Close**, **Edit**, or **Edit in Desktop** buttons in the toolbar are clicked. You can then setup event listeners (for example, `EditButtonClick`, or `PublishedAs`) to handle the button clicks and customize the authoring and publishing flow to suit the needs of your users. For example, you might follow this scenario to switch between viewing and web authoring modes.

1. Create a `<tableau-viz>` component (or `TableauViz` object), set the `suppress-default-edit-behavior` attribute to turn off the default actions that occur when the user clicks the **Edit** button, or closes, or publishes the view.

1. In the `<tableau-viz>` component, set the `src` URL and then add the `onEditButtonClicked` event listener to call your custom handler (`handleEditButtonClicked()`) when the user wants to switch to edit mode.

   ```html

    <tableau-viz
     id="viewingViz"
     src="https://myserver/t/site/workbook/sheet"
     width="800" height="600"
     suppress-default-edit-behavior
     onEditButtonClicked="handleEditButtonClicked">
    </tableau-viz>

   ```

1. Create a `<tableau-authoring-viz>` component or `TableauAuthoringViz` object, but don't specify the `src` URL to the view, so that the view doesn't appear until after the **Edit** button is clicked. You can also set the HTML style properties to hide the component or `<div>` that will contain the authoring view.

1. In your custom `handleEditButtonClicked()` method, you should assign the `src` URL to the `TableauAuthoringViz` object so that when it is rendered it shows the view. You should also set the style properties to show the authoring component and to hide the `TableauViz` object.

1. In the `<tableau-authoring-viz>` component, add the `onWorkbookPublishedAs` event attribute or property to be able to get the new URL for the saved workbook. In your custom handler for the published-as event, assign the new URL to the `TableauViz` object. 

1. In the `<tableau-authoring-viz>` component, add the `onWorkbookReadyToClose` event attribute to know when to switch back to view mode. In your custom event handler for the close event, hide the authoring component from view and show the viewing component.

For an overview of the embedded web authoring feature, see [How to Enable Self-Service Analytics in Your Application with Embedded Web Authoring](https://www.tableau.com/about/blog/2022/8/how-enable-self-service-analytics-your-application-embedded-web-authoring). For a hands-on tutorial, see [Embedded API - Web Authoring Tutorial](https://www.tableau.com/developer/learning/embedding-api-web-authoring-tutorial). And for more information, see [Embedded Web Authoring](https://help.tableau.com/current/api/embedding_api/en-us/docs/embedding_api_web_authoring.html) in the Embedding API v3 Help documentation.

Additional Considerations:

* You will likely want to allow your users to save their edits, but not have their personalized versions affect other users. In that case, you should turn off save permissions, but allow save-as permissions. It also makes sense to give each of the users a 'sandbox' project that only they can save to.

* To give your user access to personalized content that they create, your application will have to be dynamic to show users all content they have access to, not just those you create. See the page on [Using the REST API to display dynamic content]({{ site.baseurl }}/pages/03_server_management_and_restapi).

* If you are using Connected Apps for embedding views, be sure to set the scope (`scp`) claim in the JSON Web Token (JWT) to include web authoring (`tableau:views:embed_authoring`). For information about Connected Apps, see [Authentication and Single Sign-On (SSO)]({{site.baseurl}}/pages/02_auth_and_sso) and [Configure Tableau Connected Apps to Enable SSO for Embedded Content](https://help.tableau.com/current/online/en-us/connected_apps.htm).

## Embedding other Tableau Server pages

You can also use the iframe approach to embed the Tableau Server UI. This is a rarer technique, but is useful if you want to use the out-of-the-box UI, but want to achieve single sign-on from your application.

To do so, simply embed http://{server}/trusted/{ticket}/t/{site}/ into an iframe. Be sure to enable [unrestricted tickets](http://kb.tableau.com/articles/issue/login-prompt-when-embedding-server).

<br />
*Next section: [Development and Deployment]({{ site.baseurl }}/pages/07_development_and_deployment)*
