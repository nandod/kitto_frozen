## Custom user table ##

If you are using the DB Authenticator as explained [here](Config_Auth.md) and [here](http://www.ethea.it/docs/kitto/en/ref/TKDBAuthenticator.html) you can customize the SQL query used by the authenticator to fetch user data. This is useful in two kinds of (potentially overlapping) scenarios.

  1. You have an existing (legacy) database with its user/password table which you want to keep and use in your Kitto application.
  1. You want to select additional user data that you then use in views, etc.

Just add the `ReadUserCommandText` property according to the instructions in the link above (which includes a simple example). Here is a less simple one extracted from a real-world project:

```
Auth: DB
  IsClearPassword: True
  ReadUserCommandText: |
    select
      WEB_USER_NAME as USER_NAME, WEB_PASSWORD as PASSWORD_HASH, WEB_ENABLED as IS_ACTIVE,
      LAST_NAME + ' ' + NAME as FULL_NAME, ID as PERSON_ID,
      coalesce((select top 1 1 from ADMIN_OPERATOR 
                where PERSON.ID = ADMIN_OPERATOR .PERSON_ID), 0) as IS_ADMIN_OPERATOR
    from PERSON
    where (WEB_USER_NAME = :P1) and (WEB_ENABLED = 1)
```

The additional columns other than the required USER\_NAME, PASSWORD\_HASH and IS\_ACTIVE are custom columns used throughout the applications through macros such as `%Auth:IS_ADMIN_OPERATOR%`.