# Authentication and Single Sign-On

In most embedding scenarios, you will want to enable single sign-on so that the users that are signed in to your application do not have to also sign in to Tableau Server. There are a few ways to enable single sign-on to Tableau Server: **Active Directory + Kerberos**, **Active Directory with 'Enable automatic logon'**, **SAML**, **Trusted Authentication**, and **OpenID**.

*Note: This page discusses users logging in to Tableau Server. Related, but separate, is the issue of [user management](pages/03_server_management_and_restapi.md) in which you ensure all relevant users are registered with Tableau Server.*

The guidance for which single sign-on option to use is:
* If all of your users are registered in your Active Directory instance and you already use Kerberos for authentication for other applications, use Active Directory + Kerberos.
* If all of your users are registered in your Active Directory instance, but you do not use Kerberos, use Active Directory with the 'Enable automatic logon' option (which uses Microsoft SSPI).
* If you have already use SAML or OpenID in your systems, configure Tableau Server to use your existing SAML or OpenID deployment.
* In all other scenarios, Trusted Authentication is the correct choice.

## Kerberos, Active Directory, SAML, and OpenID
* To use Kerberos for SSO, you must first [configure Tableau Server to Use Active Directory](http://onlinehelp.tableau.com/current/server/en-us/config_general.htm#UserAuth) and then [configure Tableau Server to use Kerberos](http://onlinehelp.tableau.com/current/server/en-us/config_kerberos.htm)

* To use SSPI for single sign-on, check the 'Enable automatic logon' option when [configuring Tableau Server to Use Active Directory](http://onlinehelp.tableau.com/current/server/en-us/config_general.htm#UserAuth)

* [Configuring Tableau Server for Server-wide SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm)
Alternatively, if each of your clients will have their own SAML iDP, you will need to [configure Tableau Server for site-specific SAML](http://onlinehelp.tableau.com/current/server/en-us/saml_site_specific.htm)

* [Configure Tableau Server for OpenID](http://onlinehelp.tableau.com/current/server/en-us/openid_auth_server_config.htm)

## Trusted Authentication
Trusted authentication is, unlike the above options, a piece of functionality specific to Tableau Server. It allows you to trust specific machines to authenticate users on their behalf. Because the authentication happens with simple HTTP requests, it is the most versatile of the single sign-on options and can be used to integrate with, essentially, all other authentication systems.

[The Trusted Authentication documentation](http://onlinehelp.tableau.com/current/server/en-us/trusted_auth.htm) is a good resource for getting up and running, but below is a summary of the three steps in the trusted authentication workflow:

1) Configuration: This is a one-time step where you configure Tableau Server to 'trust' specific ip addresses, which will then be allowed to authenticate users. The machines to trust are usually the machines running your web application. [[Details]](http://onlinehelp.tableau.com/current/server/en-us/trusted_auth_trustIP.htm)
2) POST Request: When the user navigates to a page in your web application that contains Tableau content will make a server-side POST request to Tableau Server passing in the users's Tableau Server username, the site the content exists on, and, optionally, the client's ip address in the form data. If the ip address making the request is trusted, and the user exists in Tableau Server, Tableau Server will return a ticket. [[Details]](http://onlinehelp.tableau.com/current/server/en-us/trusted_auth_webrequ.htm)
3) Client loads the view with the ticket: Your web application now instructs the client to load the url of the desired resource, with the ticket inserted. If the ticket is valid, Tableau Server will start a session for the user and the user will see the visualization. Of course, the user does not see the HTTP requests going on behind the scenes, but simply loads a page in your application and sees embedded Tableau content without having to signin. [[Details]](http://onlinehelp.tableau.com/current/server/en-us/trusted_auth_webURL.htm)

Additional considerations:
* The trusted ticket is redeemable only once and the Tableau Server session is only valid for the visualization that was originally loaded. Therefore, your web application must request an additional ticket if refreshes the web page or navigates to a different page that contains embedded content.
* By default, tickets can be redeemed only for visualizations, and not for other content pages in Tableau Server. To enable the user to see those, you must configure [unrestricted tickets](http://kb.tableau.com/articles/issue/login-prompt-when-embedding-server). See also: the [embedding non-view content](pages/06_embedding_non_view_content.md) page in this playbook.
* If your web application has dynamic ip addresses, such that it is not feasible to trust a specific set of static ip addresses, you should create a small 'ticket requester' application that only allows requests from your web application, requests tickets from Server, and then returns them to your web application. You can then deploy this 'ticket requester' application to a static ip address.