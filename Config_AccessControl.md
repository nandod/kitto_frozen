# Access Control #

Access control is used in Kitto to manage user access to resources. Resources in this context are everything that the user can access in any way, such as models, views and fields. Each resource is uniquely identified by a string in [URI](http://en.wikipedia.org/wiki/Uniform_resource_identifier) format, and supports one or more access modes.

## Resources, Privileges and Access Modes ##

A user can perform a given operation (access mode) on an object (resource) if she has the corresponding privilege(s). For example, in order to be able to change a field's value, you need to own the MODIFY privilege on both the ModelField and the ViewField that represents it. Plus, you need the MODIFY (if editing an existing record) or ADD (if creating a new record) privileges on the ViewTable and Model.

The following tables summarize the current built-in AC-enabled resource types in Kitto and the access modes each of them supports. Since resource URIs and access modes are nothing but strings, you can add your own resource types and modes if you want, and hook them to the system

**Resource Type:** Model

**Sample URI:** `metadata://Model/Party`

| **Access Mode** | **Meaning** |
|:----------------|:------------|
| READ          | User can see the data |
| ADD           | User can add new records |
| MODIFY        | User can modify existing records |
| DELETE        | User can delete records |

**Resource Type:** ModelField

**Sample URI:** `metadata://Model/Party/Party_Name`

| **Access Mode** | **Meaning** |
|:----------------|:------------|
| READ          | User can see the data (otherwise the particular field/column is not visible) |
| MODIFY        | When editing or adding records, user can set the field's value |

---

When you don't grant MODIFY access to particular fields to a user that can add records, you must make sure that these fields, which will be read-only for the user, receive suitable default or otherwise computed values.

---


**Resource Type:** View
**Sample URI:** `metadata://View/Parties`

| **Access Mode** | **Meaning** |
|:----------------|:------------|
| VIEW          | User sees the view (in a menu or toolbar, for example) |
| RUN           | User can launch the view by clicking it (otherwise the item will look disabled) |

---

If you want a user to see a feature that is not available to him, then you grant the VIEW mode but not the RUN mode to the relevant view.
If you want to completely hide a feature, you grant neither of them.

---


**Resource Type:** ViewTable (part of a DataView)

**Sample URI:** `metadata://View/Parties` `metadata://View/Parties/Invitation`

| **Access Mode** | **Meaning** |
|:----------------|:------------|
| READ          | User can see the data |
| ADD           | User can add new records |
| MODIFY        | User can modify existing records |
| DELETE        | User can delete records |

---

Please note that a DataView's MainTable shares the same URI as the view. IOW, you don't need to individually grant privileges for a DataView and its MainTable.

---

You need to grant privileges both at the view and model level for them to be effective. This is so to allow you to make different views for different kinds of users based on the same models.

---


**Resource Type:** ViewField (a field in a ViewTable)

**Sample URI:** `metadata://View/Parties/Party_Name`

| **Access Mode** | **Meaning** |
|:----------------|:------------|
| READ          | User can see the data (otherwise the particular field/column is not visible) |
| MODIFY        | When editing or adding records, user can set the field's value |

## Access Controllers ##

Access control is implemented by a class that you can optionally enable. By default, the Null access controller class is used, which always grants access to everything. You can use a predefined access controller or create your own.

In order to enable an access controller you need to:

  * Specify its name and settings in the config file.
  * Include/use the relevant unit in your project.

Currently Kitto includes only one access controller (besides the default Null access controller), called the [DB Access Controller](http://www.ethea.it/docs/kitto/en/ref/TKDBAccessController.html), defined in unit Kitto.AccessControl.DB, which implements a simplified [RBAC (Role Based Access Control)](http://en.wikipedia.org/wiki/Role-based_access_control) strategy.

Example:
```
AccessControl: DB
  ReadRolesCommandText: |
    select ROLE_NAME from MY_USER_ROLES
    where USER_NAME = :USER_NAME or USER_NAME = '*'
    order by ROLE_NAME
```

## Users and Roles ##

The DB Access Controller uses tables in the database for defining users, roles and privileges. Please refer to [this page](http://www.ethea.it/docs/kitto/en/ref/TKDBAccessController.html) for details.

A privilege grants or revokes access to a class of resource URIs, identified by matching each URI against a pattern stored in the privilege, in one or more access modes. Here are a few examples of privileges:

| **Pattern**                  | **Matches** |
|:-----------------------------|:------------|
| `metadata://Model/*`       | All models and model fields. |
| `metadata://Model/Party/*` | All fields of model Party. |
| `metadata://Model/*/*`     | All fields of all models. |
| `metadata://View/*`        | All views, view tables and fields. |
| `~metadata://View/*`       | Everything but views. |

---

Be careful when matching patterns and negated patterns (through the ~ introducer) as the implications are often convoluted.

---

You can also uses a [regular expression (regex)](http://en.wikipedia.org/wiki/Regular_expression) as a pattern, provided you add the 'REGEX:' prefix so that Kitto knows how to interpret the rest. You can use the '~REGEX:' if you want to negate the regex instead.

---


A role is a container of related privileges. Each user can have many roles, which are all active at the same time. Users and roles are identified by unique names of your choosing.

When defining your access control strategy, you would normally want to group privileges into roles and then assign as many roles as required to each user.

## Additional features ##

All Access controllers (including the Null AC) support logging. When logging to file is enabled, the AC logs for every request received a timestamp, the user, the resource URI, the access mode and the result (1 for access granted, 0 for access denied). This is convenient both to troubleshoot privilege definition issues and to see how resource URIs are built so you can create correct matching patterns.

Example:
```
AccessControl: Null
  Log:
    FileName: %HOME_PATH%AC_log.txt
    # By default fields are not delimited
    # and separated by the Tab character.
    FieldSeparator: ,
    # Only the first character is used as delimiter.
    FieldDelimiter: "
    # By default the first line output in a file
    # contains field titles and not data.
    IncludeHeader: False
```