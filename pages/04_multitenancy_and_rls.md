# Multi-tenancy and Row-level Security

Scaling an embedded analytics deployment of Tableau often means providing content to different groups of users. There is a security implication of making sure the different groups do not see each others' data (and metadata). There is also a deployment and maintenance consideration: Common dashboards should have to be built only once, even if the different groups of users access different data with those common dashboards.

### Multi-tenancy and Sites
[Sites](http://onlinehelp.tableau.com/current/server/en-us/sites_intro.htm), in Tableau Server, act as a logical firewall. If you create a site for each client or company that will access your Tableau content, you can be assured that users on one site will not learn about users or access content on another site. 

The tradeoff from that assurance is that it can make content management a bit harder. Below, we discuss the idea of creating a single workbook that is filtered to the correct data, based on who is logged in to Tableau Server. If you were using sites, however, you would have to publish that workbook once per site. [The REST API](/03_server_management_and_restapi.md) can help automate the management of content across sites, however.

The alternative to managing content via sites is to use Projects and Permissions. The benefit is simpler content deployment and management. The downside is that you will have to pay extra care to ensure the Projects and Permissions are set up correctly. In addition, there are certain pages in the Tableau Server UI that allow users to see information about other users on their site. Therefore, if your users will be accessing both embedded content *and* accessing the Tableau Server UI directly, using Projects becomes less attractive.

## Row-level Security
Row-level Security is both a security and content management concern. The simplest way to make sure users only see their data is to manually build a dashboard for each of them. This is not scalable, so Tableau provides a variety of ways to build a single dashboard that is filtered to the correct data. The correct method to use depends on your data environment. Here is some guidance (details for each of the methods are below):
* If you have a separate database for each tenant, build a template dashboard and use the Document API to clone the dashboard for each tenant.
* If your tenants share a database and there is filtering/entitlement logic to separate who should see which row, use user filters.
* Use Initial SQL + in-database security if you already have in-database security set up *and* plan on using live connections to the database

###  Template Dashboard + Document API
[The Document API](https://github.com/tableau/document-api-python) is a Python API for modifying and querying Tableau content files (TWB/TWBX, TDS/TDSX). If your data environment is such that each tenant has their own database (and they are each structurally similar), then you can use the Document API to create a single workbook for each of your tenants. The workflow is:

First, build a 'template' workbook that points at a single database. Build the necessary visualizations and dashboards.
Then, create a script that:
1) For each tenant, uses the Document API to change the database connection information, and save a copy. [Sample Script](https://github.com/tableau/document-api-python/blob/master/samples/replicate-workbook/replicate_workbook.py)
2) Uses the [REST API (or Server Client Library)](/03_server_management_and_restapi.md)  to publish each tenant's workbook to Server.

The above is all that is necessary if your workbooks have 'embedded datasources' (the datasources haven't been published separately). If you do choose to [publish the datasources to take advantage of Tableau Server's Data Server](https://onlinehelp.tableau.com/current/pro/desktop/en-us/publish_datasources.html), the Document API will still work. You will just have to apply the above workflow first to the datasource(s) and then to the workbooks:

First, build a 'template' datasource, publish it, and build a 'template' workbook that points at the published datasource.
Then, create a script that:
1) For each tenant, uses the Document API to change the database connection information in the TDS (datasource file), and save a copy.
2) Uses the [REST API (or Server Client Library)](/03_server_management_and_restapi.md) to publish each tenant's datasource to Server. The datasource will now have a connection string that you can use in the workbooks...
3) For each tenant, for each workbook, uses the Document API to change the published datasource connection information, and save a copy.
4) Uses the [REST API (or Server Client Library)](/03_server_management_and_restapi.md) to publish each all of the workbooks to Server.

### User Filters
If your tenants 'share' a database server, and there is logic in the database that determines which users can see which rows, you can build your datasources and workbooks using 'User Filters' which is a way to filter based on which user is currently logged in to Tableau Server.

Essentially, when connecting to your data in Tableau Desktop you will JOIN the tables containing the data necessary for analaysis to the entitlements table so that each row has a column ([Owner] in the below example) specifying which Tableau Server user it belongs to. Then you can create a calculation `username() = [Owner]` and filter to `True`.

See also:
Link | Description
---- | -----------
[Setting Up User Filters](https://onlinehelp.tableau.com/current/pro/desktop/en-us/publish_userfilters_create.html#dynamic) | Knowledge base article on setting up user filters using the username() method
[Securing User Filters](https://onlinehelp.tableau.com/current/pro/desktop/en-us/publish_userfilters_create.html#publish-user-filters) | Knowledge base article on creating your user filters at the data source level so that they are tamper-proof
[Database Entitlements Strategy](https://tableauandbehold.com/2016/03/07/how-to-set-up-your-database-for-row-level-security-in-tableau/) | Blog post on database structure for row-level security
[Extract-size strategies for RLS](https://tableauandbehold.com/2016/08/08/defusing-row-level-security-in-tableau-data-extracts-before-they-blow-up-part-1/) | Blog post detailing strategies for managing size of Extracts if your entitlements logic requires many one-to-many JOINs

### In-database Security and Initial SQL
If your database has existing security, such that setting the user to run the queries as will return the correct rows for that user, you can leverage that security with Initial SQL. Because this Initial SQL command will only run when queries are sent from Tableau to the database, this option only works for live connections to the database.

To use Initial SQL: When connecting to your datasource in Tableau Desktop, click "Initial SQL". Type the correct command that will set all following queries to run as the logged-in user. For example, in Microsoft SQL Server, you might run:
```
EXECUTE AS USER = [TableauServerUser] WITH NO REVERT;
```

And now build your workbook normally. All sessions with the database will begin by running the Initial SQL that you specified.

See also:
Link | Description
---- | -----------
[Setting Up Initial SQL](hhttp://onlinehelp.tableau.com/current/pro/desktop/en-us/connect_basic_initialsql.html) | Knowledge base article on using setting up and using Initial SQL
[Preparing your DB for Initial SQL Security](https://tableauandbehold.com/2016/03/09/using-initial-sql-for/) | Blog post to help you ensure the database is ready to use Initial SQL for row-level security

## 

Next section: [Embedding in Sharepoint, Salesforce, and Mobile Apps](/05_embedding_in_other_apps.md)

Back to [User Management, Content Management & Display with the REST API](/03_server_management_and_restapi.md) or the [Table of Contents](/00_table_of_contents.md)