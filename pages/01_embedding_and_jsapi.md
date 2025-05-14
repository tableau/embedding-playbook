---
title: Embedding Tableau Views
---

The easiest method for embedding a Tableau view (dashboard or visualization) is with the copy-paste embed code. Navigate to a view on Tableau Server and copy the Embed Code from the Share toolbar option.

![Embed Code]({{ site.baseurl }}/img/embed_code.png)

Once acquired, this code can be pasted into HTML.
This method is useful for simple embedding, such as embedding into blogs or internal knowledge bases, but **only very simple embedding scenarios should use the embed code. Most deployments should instead use the Embedding API v3.** It is not significantly more work to embed with the Embedding API v3, and doing so will gain you flexibility and power in your embedded deployment.

## Using the Embedding API v3

The [Tableau Embedding API v3](https://help.tableau.com/current/api/embedding_api/en-us/index.html) provides the latest iteration of embedding capabilities for the front-end of your application. It features a modern approach to initializing embedded content using [web components](https://www.webcomponents.org/introduction). The Embedding API v3 provides improved security and integration capabilities through Connected Apps or with an External Authorization Server (EAS), both based on the JWT standard. If you are starting a new project, we recommend using this API.

The basic embed code using the Embedding API v3 and the `<tableau-viz>` web component looks something like this:


```html
<!-- 
This <script> tag links to the Embedding API library as a JavaScript ES6 module. 
To use the library in your web application, you need to set the type attribute to 
module in the <script> tag. 
-->

<script type="module" src="https://public.tableau.com/javascripts/api/tableau.embedding.3.latest.min.js"></script>

<!-- 
Initialize the API as part of your HTML code by using the <tableau-viz> web component. 
After linking to the API library, the following code is all you need to embed a Tableau view into your HTML pages.
-->

<tableau-viz id="tableauViz"       
  src='https://public.tableau.com/views/Superstore_24/Overview'      
  height='600px' width='600px' toolbar='bottom' hide-tabs>
</tableau-viz>

```

Here's what it looks like when you use the Embedding API v3 code to embed the viz on a web page:

<HTML>
<script type="module" src="https://public.tableau.com/javascripts/api/tableau.embedding.3.latest.min.js"></script>
<div id="view"><tableau-viz id='tableauViz' src='https://public.tableau.com/views/Superstore_24/Overview' height='600px' width='600px' toolbar="bottom" hide-tabs></tableau-viz></div></HTML>

---

### Join the Tableau Developer Program

To keep up to date with future releases, join the [Tableau Developer Program](https://www.tableau.com/developer) to gain access to developer previews and demos, as well as your own Tableau Cloud Developer site.

---

## Use cases for the Embedding API v3

The Embedding API v3 provide access to a large set of capabilities that allow for granular control of interactivity with embedded views, as well as the customization needed to create a seamless analytics experience within custom apps. Common use cases include:

**Filtering and setting parameters on load** -- The options object gives you a clean interface for filtering the visualization as it loads. This is useful for loading the viz with the correct context given where the user is in your application or choices they have made.
Note: Filtering with the Embedding API v3 is **not** a security mechanism. If you wish to have tamper-proof filters applied, you should use [user filters or database security]({{ site.baseurl }}/pages/04_multitenancy_and_rls).

**Custom interfaces for interacting with the view** -- Dashboards often have elements for filtering, changing parameters, and switching tabs, amongst other things. When integrating with another application, you may want to match the look and feel of dashboard interfaces with the look and feel of the embedding application. For example, you can create a drop-down with HTML/CSS/JS that matches the style of your application and have that drop-down make JavaScript API calls to filter the viz or change a parameter.

**Custom interactions** -- Because the JavaScript API gives you programmatic control over interaction, you can combine calls to create interactions that would not be possible otherwise. For example, you could create a button that switches to a specific visualization, changes a parameter, and then selects a set of marks all with a single click by the user. Or, when a user clicks a mark on a viz, you could have that drive a parameter change on another sheet. The combination of API calls is limited only by your imagination.

**Integration with other systems** -- If a user finds an insight from a visualization, it is likely the user will want to take action on that insight elsewhere in the embedding application. With the Embedding API v3, you can query data from the visualization, listen to user events, and execute code to perform an appropriate action. This allows Tableau visual analytics to integrate within your application's workflows.

In the image below, an HR-focused dashboard allows the user to select employees for an audit. When the user is ready, he or she can click "Run Audit" to generate a non-Tableau audit based on the selections made inside the Tableau viz.

![Embed Code]({{ site.baseurl }}/img/run_audit.png)

## Embedding API and JavaScript API Resources

Link | Description
---- | -----------
[Embedding API v3 Documentaion](https://help.tableau.com/current/api/embedding_api/en-us/index.html) | Official documentation for the Embedding API v3, including overview, key concepts, and API reference
[Tableau Developer Program](https://www.tableau.com/developer) | Join the Tableau Developer Program and keep up to date with the latest news and previews of developer tools and APIs


<br />
*Next section: [Authentication and Single Sign-On]({{ site.baseurl }}/pages/02_auth_and_sso)*
