## VIEW operations

View operations are specified as a JSON object in the $VAR column

### Table data

`name*`: this is the name of the resultant table.  All tables must have a name.

`singleRow`: `true|false`.  singleRow is optional and specifies whether the table has only 1 row.
If `singleRow` is set then each sub object can be referenced directly rather than through an array reference
(e.g. tablename.field instead of tablename[0].field)

`view`: the view command specifies a source object.

Example:
```
{
  "name": "exampleTable",
  "singleRow": false,
  "view": <view source object>
}
```


### View Source Objects

A View operation is able to process field modification operators on the source table to create a
resultant table that is typically a subset of the source table.

Source data is specified as an object with the following keys:

`table*`: Required: this is the name of the source table.

`recipients`: this is an optional array specifying source objects that have recipients as their values.
For example, you might have a table that has a column for `managers` and another column for `laborers`.
Without specifying recipients everyone would have access to this information.  To filter the data for specific recipients you can use the recipients operator.
To send the data to `managers`, `recipients` would become "recipients": ["managers"].  You can specify multiple recipients.  For example,
"recipients": ["managers", "laborers"] would result in data being sent to both categories of users.

A recipient refers to either a:
* user email
* user alias
* group name

Additionally, optional field modification operators can be specified for each source table.
Keys can be filtered, renamed and sub-classed via the optional field modification operators.

`exampleSource1` table columns: | ID  | users | admins | data |

Example specification:
```
{
  "name": "exampleView1",
  "singleRow": false,
  "join": {
    "table": "exampleSource1",
    "common": "ID",
    "recipients": ["users","admins"],
    "excl": ["ID"],
    "xlt": [{"old": "users", "new": "clients"}]
  }
}

```
Resulting data set:
`exampleJoin1` table columns: | clients | admins | data |
