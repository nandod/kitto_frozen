## How to selectively disable fields in the user interface ##

Generally, Kitto hides all fields that have `IsVisible: False` (at either the model or view level) and displays all fields that have `IsReadOnly: True` (again, at either the model or view level) as read-only (non editable).

What if I want to keep a field editable while inserting a new record and read-only while editing an existing record? The classic example is when a model has a user-entered primary key, which we want the user to specify when creating a record but which we don't want to be modified afterwards.

In this case, you use the `CanInsert` and `CanUpdate` boolean properties at the model or view level. In our example we would have:

```
Fields:
  Id:
    # CanInsert is True by default - unless it's an expression field.
    CanUpdate: False
```

Please note that you can't make editable a field that naturally isn't, such as an expression field.

---

Not that we recommend using user-visible primary key. We generally prefer to generate our keys and never display them to the user.

---
