# User authentication #

User authentication in Kitto is optional. If an application needs to authenticate users, an authenticator among a selection of predefined ones can be enabled, or a custom authentication can be developed.

In order to enable an authenticator you need to:

  * Specify its name and settings in the config file.
  * Include/use the relevant unit in your project.

Standard authenticators:

| **Name** | **Unit** | **Description** |
|:---------|:---------|:----------------|
| [DB](http://www.ethea.it/docs/kitto/en/ref/TKDBAuthenticator.html) | Kitto.Auth.DB | Uses a database table of users and optionally hashed passwords |
| [DBServer](http://www.ethea.it/docs/kitto/en/ref/TKDBServerAuthenticator.html) | Kitto.Auth.DBServer | Authenticates users as DB server users. |
| [OSDB](http://www.ethea.it/docs/kitto/en/ref/TKOSDBAuthenticator.html) | Kitto.Auth.OSDB | Uses Operating System authentication |
| [TextFile](http://www.ethea.it/docs/kitto/en/ref/TKTextFileAuthenticator.html) | Kitto.Auth.TextFile | Uses a text file of users and optionally hashed passwords |

Other authentication methods will be added. Plus, it's easy to tweak existing authenticators and create inherited modified versions.

By default, the Null authenticator is used, which does not require user authentication.

## The Login dialog ##

When authentication is enabled, a Kitto application displays a Login dialog at startup, unless all required credentials are specified by other means.

---

Credentials can vary depending on the authenticator, but usually they are UserName and Password, both of type String.

---

The other available means of specifying credentials, needed in special cases, are basically two:

  * In the config file, inside a Auth/Defaults subnode.
  * In the URL, as standard URL parameters.

Default credentials can be specified also partially, in which case the Login dialog is still displayed with pre-filled fields.

Example:
```
Auth: DB
  Defaults:
    UserName: guest
    Password:
```