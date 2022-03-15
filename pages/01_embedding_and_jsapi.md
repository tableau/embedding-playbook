---
title: Embedding Views & JavaScript API Usage
---

The easiest method for embedding a Tableau view (dashboard or visualization) is with the copy-paste embed code. Navigate to a view on Tableau Server and copy the Embed Code from the Share toolbar option.

![Embed Code]({{ site.baseurl }}/img/embed_code.png)

Once acquired, this code can be pasted into HTML.
This method is useful for simple embedding, such as embedding into blogs or internal knowledge bases, but **only very simple embedding scenarios should use the embed code. Most deployments should instead use the JavaScript API or the Embedding API v3.** It is not significantly more work to embed with the JavaScript API or the Embedding API v3, and doing so will gain you flexibility and power in your embedded deployment.

## Embedding with the Embedding API v3

The Embedding API v3 is the newest and easiest way to embed Tableau views, and provides you with options for enhancements, including improved security and integration for Connected Apps and external authorization servers (EAS). The Embedding API v3 is the new, modern replacement for the JavaScript API v2. 

Available starting with Tableau 2021.4, you can use the Embedding API v3 to add a Tableau view as a web component in your HTML pages. Using the Tableau viz web component, you can complete your basic embed scenarios without having to write JavaScript code. The Embedding API v3 is currently available now for basic embedding. If you are looking to replace your embed code, or you are starting a new embedding project, you should consider using the Embedding API v3. 

The basic embed code using the Embedding API v3 and the `<tableau-viz>` web component looks something like this:


```html

<script type="module" src="https://embedding.tableauusercontent.com/tableau.embedding.3.0.0.min.js"></script>


<tableau-viz id="tableauViz"       
  src='https://public.tableau.com/views/Superstore_24/Overview'      
  height='600px' width='600px' toolbar='bottom' hide-tabs>
</tableau-viz>

```

Here's what it looks like when you use the Embedding API v3 code to embed the viz on a web page:

<HTML>
<script type="module" src="https://embedding.tableauusercontent.com/tableau.embedding.3.0.0.min.js"></script>
<div id="view"><tableau-viz id='tableauViz' src='https://public.tableau.com/views/Superstore_24/Overview' height='600px' width='600px' toolbar="bottom" hide-tabs></tableau-viz></div></HTML>

---

### Join the Tableau Developer Program

The Embedding API v3 is under active development and in future releases will provide full parity with the JavaScript API v2, giving you a rich set of features that you can access in your JavaScript or TypeScript code. To keep up to date with the coming features, join the [Tableau Developer Program](https://www.tableau.com/developer) and gain access to developer previews and demos, and your own Tableau Online Developer site.

---

## Embedding with the JavaScript API

The JavaScript API is the established way to embed views. While the Embedding API v3 is recommended for basic embed code and for new embedding projects, you should continue to use the JavaScript API for existing embed projects that have a lot of customization.

The basic embed code using the JavaScript API looks something like:

HTML:

```html
<!-- JS file to enable the JavaScript API. You can point at the
  version on public.tableau.com, online.tableau.com, or your on-prem Server -->
<script src="https://www.example.com/javascripts/api/tableau-2.js"></script>
...
<!-- Empty div where the viz will be placed -->
<div id="tableauViz"></div>
```

JavaScript:

```javascript
function initializeViz() {
  // JS object that points at empty div in the html
  var placeholderDiv = document.getElementById("tableauViz");
  // URL of the viz to be embedded
  var url = "http://public.tableau.com/views/WorldIndicators/GDPpercapita";
  // An object that contains options specifying how to embed the viz
  var options = {
    width: '600px',
    height: '600px',
    hideTabs: true,
    hideToolbar: true,
  };
  viz = new tableau.Viz(placeholderDiv, url, options);
}
```

## Use cases for the JavaScript API and the Embedding API v3

After embedding with the JavaScript API or the Embedding API, you have unlocked its capabilities which are crucial for integrating with the front-end of your application. The possibilities with the JavaScript API and Embedding API are effectively infinite, but common use cases are:

**Filtering and setting parameters on load** -- The options object gives you a clean interface for filtering the visualization as it loads. This is useful for loading the viz with the correct context given where the user is in your application or choices they have made.
Note: Filtering with the JavaScript API or Embedding API is **not** a security mechanism. If you wish to have tamper-proof filters applied, you should use [user filters or database security]({{ site.baseurl }}/pages/04_multitenancy_and_rls).

**Custom interfaces for interacting with the view** -- Dashboards often have elements for filtering, changing parameters, and switching tabs, amongst other things. When integrating with another application, you may want to match the look and feel of dashboard interfaces with the look and feel of the embedding application. For example, you can create a drop-down with HTML/CSS/JS that matches the style of your application and have that drop-down make JavaScript API calls to filter the viz or change a parameter.

**Custom interactions** -- Because the JavaScript API gives you programmatic control over interaction, you can combine calls to create interactions that would not be possible otherwise. For example, you could create a button that switches to a specific visualization, changes a parameter, and then selects a set of marks all with a single click by the user. Or, when a user clicks a mark on a viz, you could have that drive a parameter change on another sheet. The combination of API calls is limited only by your imagination.

**Integration with other systems** -- If a visualization is embedded inside another system, and a user finds an insight from a visualization, it is likely the user will want to take action on that insight elsewhere in your application. With the JavaScript API and Embedding API, you can query data on the visualization, or listen to user events, and execute code that takes the appropriate action.

In the image below, an HR-focused dashboard allows the user to select employees for an audit. When the user is ready, he or she can click "Run Audit" to generate a non-Tableau audit based on the selections made inside the Tableau viz.

![Embed Code]({{ site.baseurl }}/img/run_audit.png)

## Embedding API and JavaScript API Resources

Link | Description
---- | -----------
[Embedding API v3 Documentaion](https://help.tableau.com/current/api/embedding_api/en-us/index.html) | Official documentation for the Embedding API v3, including overview, key concepts, and API reference
[Tableau Developer Program](https://www.tableau.com/developer) | Join the Tableau Developer Program and keep up to date with the latest news and previews of developer tools and APIs
[JavaScript API v2 Documentation](http://onlinehelp.tableau.com/current/api/js_api/en-us/JavaScriptAPI/js_api.htm#) | Official documentation for the JavaScript API including an overview, tutorial, samples, explanation of key concepts, and API reference
[JavaScript API Tutorial](http://onlinehelp.tableau.com/samples/en-us/js_api/tutorial.htm) | An interactive tutorial that will walk you through the key concepts of the JavaScript API
[JavaScript API YouTube Playlist](https://www.youtube.com/watch?v=Geppur9LDnw&list=PL_qx68DwhYA8e_z9k7uoRw0zayoY35nUJ) | A series of youtube videos to get you up and running with the JavaScript API
[JavaScript API Samples](https://github.com/tableau/js-api-samples) | Official samples, created and maintained by Tableau


<br />
*Next section: [Authentication and Single Sign-On]({{ site.baseurl }}/pages/02_auth_and_sso)*
