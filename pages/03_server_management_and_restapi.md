---
title: User Management, Content Management, and Display with the REST API
---

Embedding a single visualization with the [Embedding API v3]({{ site.baseurl }}/pages/01_embedding_and_jsapi) and enabling [single sign-on]({{ site.baseurl }}/pages/02_auth_and_sso) is a crucial first step, but is only part of the story when building scaled solutions that use Tableau content as components. You often will have to use the REST API to integrate the users and content between your system and Tableau Server.

The REST API allows you to query and manage sites, users, groups, workbooks, data sources, projects, and subscriptions/schedules. The automation and integration use cases are effectively infinite, but the most common workflows it is used for in embedded analytics are:

* User creation to sync with the users in the embedding application
* Dynamic display of workbooks and data sources that a user has access to
* Publishing workbooks as the last step in a content-integration workflow

Because the REST API is called via HTTP, you can use whichever programming language is most appropriate. However, there is also the [Server Client Library](https://github.com/tableau/server-client-python) which simplifies the code required. The Server Client Library is currently only available in Python.

Most commonly, you will make REST API calls from your web application's server-side code. In some cases, you will want to make those calls client-side (for example, if you are building web pages with no control over the Server-side logic). For those scenarios, [Tableau Server enables CORS support](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api_concepts_fundamentals.htm#Enabling).

Below are high-level descriptions of the flows required to enable the most common REST API use cases, but you should [read the documentation](https://help.tableau.com/current/api/rest_api/en-us/help.htm) to learn how to make the individual calls. [The concepts](https://help.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_concepts.htm%3FTocPath%3DConcepts%7C_____0) section is a good place to start and to learn about subjects such as filtering, sorting, and paginating. Also, be sure to explore [the REST API samples repository](https://github.com/tableau/rest-api-samples).

## User Creation

Often, your application will store users. Except in the case of syncing with Active Directory, you will need to replicate those users into Tableau Server. You will likely have a script that adds all existing users to Server as a set-up step, and some code that executes when a new user is added to your system. To add a single user to Tableau Server:

1. [Sign in](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api_concepts_auth.htm)
1. [Add user to site](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api_ref_users_and_groups.htm#add_user_to_site) (If the user does not exist, they will be added to Tableau Server and to the site you specify)
1. [Sign out](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api_concepts_auth.htm)

That's all there is to adding a new user, but you will likely want to make some other calls to ensure the new user has access to the correct content. See the [Tableau REST API](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api.htm) documentation for more details and examples.

## Dynamic Display of Content

If your users have access to multiple workbooks and data sources, you will likely want to make a Table of Contents page similar to the below.

![Viz Table of Contents]({{ site.baseurl }}/img/viz_TOC.png)

And if *different* users have access to *different* sets of content, it will not make sense for the list of available content to be hard-coded. Therefore, you need to use the REST API to query the workbooks available to the logged-in user. To achieve the above, you also need to grab the thumbnails.

1. [Sign in](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api_concepts_auth.htm) (as admin)
1. [Query workbooks for user](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api_ref_workbooks_and_views.htm#query_workbooks_for_user) passing in the username of the user currently logged in to your application
1. For each workbook returned: [Grab the Preview Image](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api_ref.htm#query_view_with_preview)
1. Populate the Table of Contents with links to pages that embed the workbooks, using the preview images.
1. When the user clicks one of the links, they are brought to a page in your application that embeds the chosen dashboard. To avoid having to hard-code these embedding pages, you will likely create a dynamic web page that can be passed the workbook name and view name so that you can construct the url for the Embedding API v3 to use. The details of engineering that dynamic webpage depend on your web application and front-end framework.

## Publishing Workbooks

The typical workflow for building and publishing content is to do it manually with Tableau Desktop. You might need to publish with the REST API for a variety of reasons. For example:

* Using the [Document API]({{ site.baseurl }}/pages/04_multitenancy_and_rls) to generate workbooks from a template and then publishing each of those to Server
* Creating a script that runs on install of Server to publish a set of pre-built dashboards
* Scripting migration from site to site or server to server

Depending on the size of the workbook or data source, it may be possible to publish with a single call. If the file is large, you will need to send the file in pieces. For more details, see [the documentation on publishing with the REST API](https://help.tableau.com/current/api/rest_api/en-us/REST/rest_api_concepts_publish.htm?TocPath=Concepts%7C_____11).

<br />
*Next section: [Multi-Tenancy and Row-Level Security]({{ site.baseurl }}/pages/04_multitenancy_and_rls)*
