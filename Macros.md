# Macros #

Kitto allows you to put macros in yaml files. A macro is a symbol that is later expanded to (meaning replaced with) its actual meaning. Macros can be used in place of variables, to make certain parts of your definitions dynamic or context-sensitive. For example, if you wanted to display the current date you would use the `%DATE%` macro. If you needed the name of the currently authenticated user, you would use `%Auth:UserName%`.

## Predefined macros ##

A number of macros are predefined, meaning available by default in every Kitto application. Have a look [in the reference guide](http://www.ethea.it/docs/kitto/en/ref/EF_Macros_pas.html) for a list.

## Config macros ##

You can access the value of any key in Config.yaml (or whatever config file was used to start the application) through the syntax `%Config:Path/To/Value%`. Slashes are used to descend the yaml hierarchy.

This is useful to set two config keys to the same value without wirting it twice, or to set a view property in terms of a config setting.

## Auth macros ##

All authentication data is available as macros in the `Auth:` namespace. Most common among them are `%Auth:UserName%` and `%Auth:Password%`, but you can add your own values by customizing the auth definition in the config file.

## Database macros ##

Macros in the `DB:` namespace are expanded by the current DB adapter. This allows a certain degree of database-independence when writing, for example, a SQL expression for a model field:

```
  Expression: |
    case
      when %DB.CURRENT_DATE% - PARTY.PARTY_DATE < 0 then cast('Future' as varchar(10))
      when %DB.CURRENT_DATE% - PARTY.PARTY_DATE = 0 then cast('Today' as varchar(10))
      when %DB.CURRENT_DATE% - PARTY.PARTY_DATE = 1 then cast('Yesterday' as varchar(10))
      when %DB.CURRENT_DATE% - PARTY.PARTY_DATE between 2 and 7 then cast('Last Week' as varchar(10))
      when %DB.CURRENT_DATE% - PARTY.PARTY_DATE between 8 and 30 then cast('Last Month' as varchar(10))
      else cast('Older' as varchar(10))
    end
```
The code above will work on both Firebird and SQL Server databases because the `%DB:CURRENT_DATE%` macro is expanded to `current_date` in the former case and to `getdate()` in the latter.

---

These are special macros that are only valid inside SQL command texts.

---


## Adding custom macros ##

You can add custom macros in your application by creating and registering a macro expander class. A macro expander is a class inherited from EF.Macros.TEFMacroExpander. Use the following (which is a real pre-defined macro expander, probably the simplest one) as an example and code template:

```
uses
  EF.Macros;

type
  TEFEntityMacroExpander = class(TEFMacroExpander)
  strict protected
    function InternalExpand(const AString: string): string; override;
  end;

implementation

{ TEFEntityMacroExpander }

function TEFEntityMacroExpander.InternalExpand(const AString: string): string;
begin
  Result := inherited InternalExpand(AString);

  Result := ExpandMacros(Result, '%TAB%', #9);
  Result := ExpandMacros(Result, '%SPACE', ' ');
end;

initialization
  TEFMacroExpansionEngine.Instance.AddExpander(TEFEntityMacroExpander.Create);
```