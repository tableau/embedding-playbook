# Development and Deployment
Developing content in Tableau does not require traditional source control or dev-test-prod techniques. However, you may wish to integrate the Tableau content development and deployment into your existing development systems. In this article, we specifically cover dev-test-prod and source control.

### Dev-Test-Prod
If you have a workbook that is in development or test modes and needs to be promoted to the next steps, you may have to do one or both of 1) point the workbook at a different datasource, 2) promote the workbook inside Tableau Server

To point the workbook to a different datasource, you can use the [Document API](https://github.com/tableau/document-api-python) to modify the connection string of the workbook. If the workbook is already on Server, you will need to:
1) Use the [REST API](./03_server_management_and_restapi.md) to download the workbook
2) Use the Document API to change the connection string
3) Republish the workbook with the REST API

The technique for promoting inside of Tableau Server depends on whether you separate your environments with projects, with sites, or with different servers.
To promote from one project to another, simply use the [REST API Update Workbook endpoint](http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Update_Workbook%3FTocPath%3DAPI%2520Reference%7C_____87) to change the project.

To promote from one site to another:
1) Download with the REST API
2) (Optional) Use the REST API to delete the workbook from the old environment
3) Use the Document API to change the connection string (if necessary)
2) Publish with the REST API to the new site

To promote from one server to another, use the site-to-site technique, but you will have to sign in to the new server with the REST API to publish.

Here are some samples:
* [Migrate projects using the Python Server Client Library](https://github.com/tableau/server-client-python/blob/master/samples/move_workbook_projects.py)
* [Migrate sites using the Python Server Client Library](https://github.com/tableau/server-client-python/blob/master/samples/move_workbook_sites.py)
* [C# tool for migrating with the REST API](https://github.com/tableau/TabMigrate)
* [Modify connection information with the Document API](https://github.com/tableau/document-api-python/blob/master/samples/replicate-workbook/replicate_workbook.py)

### Source Control
Tableau Server has built in [revision history](https://onlinehelp.tableau.com/current/server/en-us/revision_history_maintain.htm), but you must enable it for a given site. Once enabled, it stores each published version of a workbook or data source, with full access to download or restore back to a given point. Revision history can also be accessed via the Tableau [REST API](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_ref.htm#Get_Workbook_Revisions%3FTocPath%3DAPI%2520Reference%7C_____42), so this functionality can be integrated in with other tools you have.

The Tableau content files (TDS and TWB) are XML files and thus can be easily stored in any sort of external revision control system. When you create an extract (TDE) or save a packaged workbook (TDSX or TWBX file), the files are binary and thus source control is less useful. Doing diff processes on workbook and data source files is not particularly useful, since there is no real easy way to merge changes with Tableau files, but having a check-in process to manage who is working on a workbook or data source can definitely have advantages.

###
For a more detailed discussion of the above topics, see [this blog post](https://tableauandbehold.com/2017/03/07/developing-and-deploying-tableau-content/)

## 

Back to [Embedding Non-View Content](./06_embedding_non_view_content.md) or the [Table of Contents](./00_table_of_contents.md)