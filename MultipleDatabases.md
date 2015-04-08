# Database Routing #

Kitto allows you to work with more than one databases at a time through the database routing feature. The basic use of this feature is explained [here](Models.md).

## Database routing at the View level ##

Beside linking a model to a particular database, you can do database routing at the DataView level, using exactly the same syntax:

```
DatabaseRouter: Static
  DatabaseName: Other
```

This is useful in case you have multiple databases with similar structure and want to define a single model with different views each linked to a different database.

## Other uses of database routing ##

Database routing is enabled in other places, always through the same syntax. These places are:

| `Config.yaml/AccessControl` | For database-based access controllers, defines the database holding the AC data. |
|:----------------------------|:---------------------------------------------------------------------------------|
| `Config.yaml/Auth` | For database-based authenticators, defines the database holding the auth data (user names and passwords, generally.|
| DynaList filters in a [List](List.md) controller| You can fetch the list of values for a list filter from a different database if nedded. |

Generally, you can use database routing everywhere a database connection is implied.

## Advanced: Customizing database routing ##

All examples of database routing we have seen so far have been using the `Static` router, but you can create your own router if you have a dynamic rule that matches models to databases (for example, a naming convention). Creating your own router involves looking into the `Kitto.DatabaseRouter` unit and using the existing `Static` router as an example. It all boils down to inheriting a new class from `TKDatabaseRouter` and overriding the `InternalGetDatabaseName` method to implement said rule. Your new router can have as many parameters as needed. Everything below the `DatabaseRouter` node is passed to the `InternalGetDatabaseName` method at run time.

Don't forget to call `TKDatabaseRouterRegistry.Instance.RegisterClass` in your initialization section (and the symmetrical `TKDatabaseRouterRegistry.Instance.UnregisterClass` in the finalization section) in order to make your custom database router available to your application.