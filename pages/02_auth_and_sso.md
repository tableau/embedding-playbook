---
title: Authentication and Single Sign-On (SSO)
---

In most embedding scenarios, you will want to enable single sign-on so that the users that are signed in to your application do not have to also sign into Tableau Server or Tableau Online. There are various options to enable single sign-on (SSO) to Tableau.

**Note:** This page discusses users logging into Tableau Server and  Tableau Online. Related, but separate, is the issue of [user management]({{ site.baseurl }}/pages/03_server_management_and_restapi) in which you ensure all relevant users are registered and provisioned with Tableau.

The guidance for which single sign-on option to use is:

* **Connected Apps:** Use Connected Apps if you want to facilitate an explicit trust relationship between Tableau Online or Tableau Server and external applications where Tableau content is embedded. The trust relationship is established and verified through an authentication token in the [JSON Web Token (JWT) standard](https://datatracker.ietf.org/doc/html/rfc7519).

* **External Authorization Servers (EAS):** Use EAS if you prefer to establish a trust relationship between Tableau Server and an identity provider you've already configured for Tableau Server. A standard OAuth flow is used to provide your users a single sign-on experience to Tableau content embedded in your external applications.

* **Trusted Authentication:** Use Trusted Authentication if you wish to establish trust between Tableau Server and one or more web servers using an IP allowlist. Until the release of Connected Apps and EAS, Trusted Authentication was the most commonly implemented single sign-on solution. If advanced JavaScript API v2 capabilities are required, Trusted Authentication will still be the best fit.

* **Active Directory + Kerberos:** If all of your users are registered in your Active Directory instance and you already use Kerberos for authentication for other applications, use Active Directory + Kerberos.
* **Active Directory + 'Enable automatic logon':** If all of your users are registered in your Active Directory instance, but you do not use Kerberos, use Active Directory with the 'Enable automatic logon' option (which uses Microsoft SSPI).
* **SAML or OpenID:** If you have already use SAML or OpenID in your systems, configure Tableau Server to use your existing SAML or OpenID deployment.

## Connected Apps and External Authorization Servers (EAS)

With Connected Apps (CA) and External Authorization Server (EAS), you have two modern options to implement seamless SSO authentication for embedded Tableau views. You can either setup a trust relationship between Tableau Server, or Tableau Online, and your external application (CA) using an authentication token in the JWT standard. Or you can establish a trust relationship between Tableau Server and an identity provider (EAS) to implement a standard OAuth flow. Both options provide additional security and control scopes over Trusted Authentication. To leverage either of these methods, you must use Tableau 2021.4 (or later) and the Embedding API v3 to embed your views.

### Connected Apps

For information about using connected apps for embedding views from Tableau Online, see [Configure Tableau Connected Apps to Enable SSO for Embedded Content](https://help.tableau.com/current/online/en-us/connected_apps.htm). For information about setting up a connected app on Tableau Server or Tableau Online using the Tableau REST API, see the [Connected App Methods](https://help.tableau.com/v2021.4/api/rest_api/en-us/REST/rest_api_ref_connected_app.htm).

Here is a short summary of the steps you need to take. There are four parts to enabling your embedded view as a connected app.

1. As a Tableau site administrator, login in to Tableau Online and create a new connected app. Or for Tableau Server or Tableau Online, use the REST API connected apps methods to create a new connected app). Make note of the client ID, as you will need this to create the JWT.

1. Generate the secret(s) for the connected app. Make note of this secret ID and secret value as you will need these when you create the JWT.

1. Configure the web server that hosts your embedded application to generate the (JWT). The JWT is generated dynamically for each user. There are JWT libraries and packages in various languages that you can use to build the JWT.

1. After you have the JWT, you need to pass this value to the Tableau viz web component `<tableauViz>`. Once configured, users can securely view embedded content in your application without going through login screens.

### External Authorization Servers

If you are using an IdP on Tableau Server to authenticate users, you can use an external authorization server (EAS). The EAS must be set up to provide a JSON web token (JWT) for each user. You use the JWT when you embed the Tableau view as a web component in your application. When the embedded content is loaded, the standard OAuth flow is used. After users sign in to the IdP, they are automatically signed in to Tableau Server. For information, see [Register EAS to Enable SSO for Embedded Content (Linux)](https://help.tableau.com/current/server-linux/en-us/connected_apps_eas.htm) or [Register EAS to Enable SSO for Embedded Content (Windows)](https://help.tableau.com/current/server/en-us/connected_apps_eas.htm).

### Add the JWT to the Tableau viz component

Whether you are configuring your embedded web application to use EAS for Tableau Server, or as a connected app on Tableau Online or Tableau Server, you need to explicitly pass the JWT that is generated by the EAS or by your web server to the `<tableauViz>` web component.  You do this using the `token` attribute.

For example, if you programmatically build the JWT for each user and assign it to a variable `JWT`, you might use a template literal to reference the JWT on your HTML page.

```html

<tableau-viz id="tableauViz"
  src='https://your-tableau-server/views/my-workbook/my-view'
  token="${JWT}">
</tableau-viz>


```

## Trusted Authentication

Trusted authentication is a piece of functionality specific to Tableau Server. It allows you to trust specific machines to authenticate users on their behalf. Because the authentication happens with simple HTTP requests, it is a versatile single sign-on option and can be used to integrate with, essentially, all other authentication systems or web auth flows.

[The Trusted Authentication documentation](https://onlinehelp.tableau.com/current/server/en-us/trusted_auth.htm) is a good resource for getting up and running, but below is a summary of the three steps in the trusted authentication workflow:

1. Configuration: This is a one-time step where you configure Tableau Server to 'trust' specific IP addresses, which will then be allowed to authenticate users. The machines to trust are usually the machines running your web application. [[Details]](https://onlinehelp.tableau.com/current/server/en-us/trusted_auth_trustIP.htm)
1. POST Request: When the user navigates to a page in your web application that contains Tableau content, the web application will make a server-side POST request to Tableau Server passing in the users's Tableau Server username, the site the content exists on, and, optionally, the client's IP address in the form data. If the IP address making the request is trusted, and the user exists in Tableau Server, Tableau Server will return a ticket. [[Details]](https://onlinehelp.tableau.com/current/server/en-us/trusted_auth_webrequ.htm)
1. Client loads the view with the ticket: Your web application now instructs the client to load the url of the desired resource, with the ticket inserted. If the ticket is valid, Tableau Server will start a session for the user and the user will see the visualization. Of course, the user does not see the HTTP requests going on behind the scenes, but simply loads a page in your application and sees embedded Tableau content without having to signin. [[Details]](https://onlinehelp.tableau.com/current/server/en-us/trusted_auth_webURL.htm)

Additional considerations:

* A common desire is to use a single 'service' account to authenticate the users. This is not a recommended approach, because it does not allow you to apply [data security]({{ site.baseurl }}/pages/04_multitenancy_and_rls) or to track usage on a per-user basis.
* The trusted ticket is redeemable only once within three minutes of being issued and establishes a Tableau Server session for the user. The session allows the user to access any of the views that they have access to, as determined by the user and content permissions on the server. For more information, see [Trusted Authentication](https://help.tableau.com/current/server/en-us/trusted_auth.htm).
* By default, tickets can be redeemed only for embedded visualizations, and not for other content pages in Tableau Server. To enable the user to see those, you must configure [unrestricted tickets](https://kb.tableau.com/articles/issue/login-prompt-when-embedding-server). See also: the [embedding non-view content]({{ site.baseurl }}/pages/06_embedding_non_view_content) page in this playbook.
* If your web application has dynamic IP addresses, such that it is not feasible to trust a specific set of static IP addresses, you have a couple of options. You could create a small 'ticket requester' application that only allows requests from your web application. The 'ticket requester' requests tickets from the server, and then returns them to your web application. You can then deploy this 'ticket requester' application to a static IP address. Or you could consider leveraging one of the other authentication mechanisms listed above that do not depend on an IP allowlist.

## Kerberos, Active Directory, SAML, and OpenID

* To use Kerberos for SSO, you must first [configure Tableau Server to Use Active Directory](https://onlinehelp.tableau.com/current/server/en-us/config_general.htm#UserAuth) and then [configure Tableau Server to use Kerberos](https://onlinehelp.tableau.com/current/server/en-us/config_kerberos.htm)

* To use SSPI for single sign-on, check the 'Enable automatic logon' option when [configuring Tableau Server to Use Active Directory](https://onlinehelp.tableau.com/current/server/en-us/config_general.htm#UserAuth)

* [Configuring Tableau Server for Server-wide SAML](https://onlinehelp.tableau.com/current/server/en-us/config_saml.htm)
Alternatively, if each of your clients will have their own SAML iDP, you will need to [configure Tableau Server for site-specific SAML](https://onlinehelp.tableau.com/current/server/en-us/saml_site_specific.htm)

* [Configure Tableau Server for OpenID](https://onlinehelp.tableau.com/current/server/en-us/openid_auth_server_config.htm)

<br />
*Next section: [User Management, Content Management & Display with the REST API]({{ site.baseurl }}/pages/03_server_management_and_restapi)*
