---
title: User Management, Content Management, and Display with the REST API
---

Embedding a single visualization with the [JavaScript API]({{ site.baseurl }}/pages/01_embedding_and_jsapi) and enabling [single sign-on]({{ site.baseurl }}/pages/02_auth_and_sso) is a crucial first step, but is only part of the story when building scaled solutions that use Tableau content as components. You often will have to use the REST API to integrate the users and content between your system and Tableau Server.

The REST API allows you to query and manage sites, users, groups, workbooks, datasources, projects, and subscriptions/schedules. The automation and integration use cases are effectively infinite, but the most common workflows it is used for in embedded analytics are:

* User creation to sync with the users in the embedding application
* Dynamic display of workbooks and datasources that a user has access to
* Publishing workbooks as the last step in a content-integration workflow

Because the REST API is called via HTTP, you can use whichever programming language is most appropriate. However, there is also the [Server Client Library](https://github.com/tableau/server-client-python) which simplifies the code required. The Server Client Library is currently only available in Python, but more languages are coming soon.

Most commonly, you will make REST API calls from your web application's server-side code. In some cases, you will want to make those calls client-side (for example, if you are building web pages with no control over the Server-side logic). For those scenarios, [Tableau Server enables CORS support](https://onlinehelp.tableau.com/current/api/rest_api/en-us/REST/rest_api_concepts_fundamentals.htm#Enabling).

Below are high-level descriptions of the flows required to enable the most common REST API use cases, but you should [read the documentation](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm) to learn how to make the individual calls. [The concepts](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_concepts.htm%3FTocPath%3DConcepts%7C_____0) section is a good place to start and to learn about subjects such as filtering, sorting, and paginating. Also, be sure to explore [the REST API samples repository](https://github.com/tableau/rest-api-samples).

## User Creation
Often, your application will store users. Except in the case of syncing with Active Directory, you will need to replicate those users into Tableau Server. You will likely have a script that adds all existing users to Server as a set-up step, and some code that executes when a new user is added to your system. To add a single user to Tableau Server:

1. [Sign in](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Sign_In%3FTocPath%3DAPI%2520Reference%7C_____77)
1. [Add user to site](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Add_User_to_Site%3FTocPath%3DAPI%2520Reference%7C_____9) (If the user does not exist, they will be added to Tableau Server and to the site you specify)
1. [Sign out](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Sign_Out%3FTocPath%3DAPI%2520Reference%7C_____78)

That's all there is to adding a new user, but you will likely want to make some other calls to ensure the new user has access to the correct content. For example, you may [Add the user to group(s)](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Add_User_to_Group%3FTocPath%3DAPI%2520Reference%7C_____8). If the group(s) you add the user to have the correct permissions for that user, that may be sufficient, but if not, you can [add workbook permissions](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Add_Workbook_Permissions%3FTocPath%3DAPI%2520Reference%7C_____11), [add project permissions](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Add_Project_Permissions%3FTocPath%3DAPI%2520Reference%7C_____6), or [add datasource permissions](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Add_Datasource_Permissions%3FTocPath%3DAPI%2520Reference%7C_____3).

## Dynamic Display of Content
If your users have access to multiple workbooks and datasources, you will likely want to make a Table of Contents page similiar to the below.

![Viz Table of Contents]({{ site.baseurl }}/img/viz_TOC.png)

And if *different* users have access to *different* sets of content, it will not make sense for the list of available content to be hard-coded. Therefore, you need to use the REST API to query the workbooks available to the logged-in user. To achieve the above, you also need to grab the thumbnails.

1. [Sign in](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Sign_In%3FTocPath%3DAPI%2520Reference%7C_____77) (as admin)
1. [Query workbooks for user](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Query_Workbooks_for_User%3FTocPath%3DAPI%2520Reference%7C_____71) passing in the username of the user currently logged in to your application
1. For each workbook returned: [Grab the Preview Image](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Query_View_Preview_Image%3FTocPath%3DAPI%2520Reference%7C_____63)
1. Populate the Table of Contents with links to pages that embed the workbooks, using the preview images.
1. When the user clicks one of the links, they are brought to a page in your application that embeds the chosen dashboard. To avoid having to hard-code these embedding pages, you will likely create a dynamic web page that can be passed the workbook name and view name so that you can construct the url for the JavaScript API to use. The details of engineering that dynamic webpage depend on your web application and front-end framework.

## Publishing Workbooks

The typical workflow for building and publishing content is to do it manually with Tableau Desktop. You might need to publish with the REST API for a variety of reasons. For example:

* Using the [Document API]({{ site.baseurl }}/pages/04_multitenancy_and_rls) to generate workbooks from a template and then publishing each of those to Server
* Creating a script that runs on install of Server to publish a set of pre-built dashboards
* Scripting migration from site to site or server to server

Depending on the size of the workbook or datasource, it may be possible to publish with a single call. If the file is large, you will need to do a send the file in pieces. For more details, see [the documentation on publishing with the REST API](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_concepts_publish.htm%3FTocPath%3DConcepts%7C_____11)

<br />
*Next section: [Multi-Tenancy and Row-Level Security]({{ site.baseurl }}/pages/04_multitenancy_and_rls)*
