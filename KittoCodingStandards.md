# Introduction #

This page lists coding standards to be used for writing Kitto code. This is meant to keep a coherent code base, style-wise, throughout the framework.

# Indentation and structure #

Indent is two spaces. No tabs.
Code lines may be long up to about 80-100 characters. Continuation lines are indented. Do not ever wrap after ":" or ")", and before ";" or "(".

```
    procedure Load(const ADBConnection: TEFDBConnection;
      const ACommandText: string; const AAppend: Boolean = False); overload;
```

---

The "begin" and "end" keywords are always on their own line. Same for "else" (exception: "else if").

```
    for I := 1 to Length(Result) do
    begin
      if Result[I] = '.' then
        Result[I] := ','
      else if Result[I] = ',' then
        Result[I] := '.';
    end;
```

---

Assert() calls should be abundant at the beginning of routines to perform basic checks on input arguments and other preconditions (after the "inherited" call) and separated by actual statements by a blank line.
Assert() calls at the end of a function to check for assigned/valid Result and other postconditions are welcome where applicable (again, after a blank line).
Use blank lines liberally to separate blocks of code but beware of monster methods.

```
procedure TKMetadataCatalog.Close;
var
  I: Integer;
  LObject: TKMetadata;
begin
  Assert(IsOpen);

  for LObject in FNonpersistentObjects.Values do
    LObject.Free;
  FNonpersistentObjects.Clear;

  for I := FDisposedObjects.Count - 1 downto 0 do
  begin
    FDisposedObjects[I].Free;
    FDisposedObjects.Delete(I);
  end;
  FDisposedObjects.Clear;

  for I := FIndex.Count - 1 downto 0 do
  begin
    FIndex.Objects[I].Free;
    FIndex.Delete(I);
  end;
  FIndex.Clear;
  FreeAndNil(FIndex);
end;
```

---

Use xmldoc comments to document what a piece of code does. Use in-code comments to document unobvious things ONLY, such as why something had to be done in a certain way.

```
    /// <summary>
    ///   Marks the object as disposed and removes it from the index (but
    ///   doesn't free it). The file is then deleted when SaveAll is called.
    /// </summary>
    /// <param name="AObject">
    ///   Object to be disposed.
    ///  </param>
    procedure MarkObjectAsDisposed(const AObject: TKMetadata);
```

If you find yourself writing a comment describing what the following piece of code does, consider refactoring out the code block in a new method (or local routine) and let the method name be the comment you were about to write.

```
function TKMetadataCatalog.ObjectByNode(const ANode: TEFNode): TKMetadata;
begin
  Result := FindObjectByNode(ANode);
  if Result = nil then
    if Assigned(ANode) then
      ObjectNotFound(ANode.Name + ':' + ANode.AsString)
    else
      ObjectNotFound('<nil>');
end;
```

---

In uses clauses:
  * Always wrap and indent after the "uses" keyword.
  * Order units according to the following scheme whenever possible: first RTL units, then library units, then EF units, then Kitto units, then Kitto.Ext units, each group in its own line.

```
uses
  Math, FmtBcd, Variants, StrUtils,
  EF.StrUtils, EF.Localization, EF.JSON,
  Kitto.Types, Kitto.Config;
```

---

All function arguments should be declared "const" when possible (exceptions tolerated for arguments of anonymous methods of TProc<> and TFunc<> type).
Output arguments are "out" and input/output arguments are "var". No exceptions.

```
function TEFYAMLParser.ParseLine(const ALine: string; out AName, AValue: string): Boolean;
var
  LLine: string;
  P: Integer;
  LIndent: Integer;
```

If you feel you need to change a function argument locally, declare a local variable instead.

---

Always wrap and indent after "begin" and "var".

---

Always use try/finally blocks whenever a symmetric operation must take place (such as Create/Free, Disable`*`/Enable`*`, etc.).

---

Don't reference units from upper layers. A Kitto.Metadata.`*` unit must not reference any Kitto.Ext.`*` unit. A EF.`*` unit must not reference any Kitto.`*` units, and so on.

---

In class declarations, prefer "strict private" and "strict protected" specifiers. Don't use the legacy protected hacks (unit-level visibility) unless absolutely needed.
Define a "public" section with all the public members meant to be used by the class user and put it at the end of the class declaration.
Plus, (this might seem weird) define an additional "public" section and put it before the other one if you need to add scaffolding or methods not meant for public consumption, such as overrides of AfterConstruction or Destroy.

```
  TKDatabaseRouter = class(TEFComponent)
  strict protected
    function InternalGetDatabaseName(const ACallerContext: TObject;
      const AParams: TEFTree): string; virtual; abstract;
  public
    procedure AfterConstruction; override;
    destructor Destroy; override;
  public
    /// <summary>
    ///   Returns a database name suitable for the specified context and
    ///   params.
    /// </summary>
    function GetDatabaseName(const ACallerContext: TObject;
      const AParams: TEFTree): string;
  end;
```

# Naming and capitalization #

All framework units are named Kitto.`*` or Kitto.X.`*` where X is an area name (such as Metadata, Ext, Auth, etc.). Units outside an area are considered of general use (such as Kitto.Types).
Units named EF.`*` are lower level and not Kitto-specific.

---

Type naming rules reflect Delphi's classic naming. T prefix for classes, E for exceptions, I for interfaces, and CamelCaps everywhere. All Kitto classes uses a TK prefix. All EF classes use a TEF prefix.
Kitto ext-related classes, such as controllers, use a TKExt prefix.

---

Data values (variables, class fields, function arguments) all have a prefix:
  * L for locals.
  * A for function arguments.
  * F for class fields.
The only exception is the I local variable which may be used as loop counter (a standard LSomething is also acceptable for this purpose).
Extremely rare implementation-level global variables might be prefixed with "`_`".

No interface-level global variables, ever.

---

All keywords are all lowercase; "string" is a keyword; "Integer" and "Boolean" are not.

# Compiler #

No warnings should survive past commit. All committed code should be warning-free unless it's a Work In Progress.

Hints are treated the same as warnings.

Assertions are always compiled in, **especially** in Release builds.