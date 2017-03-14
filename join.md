## JOIN operations

Join operations are specified as a JSON object in the $VAR column

### Table data

`name*`: this is the name of the resultant table.  All tables must have a name.

`singleRow`: `true|false`.  singleRow is optional and specifies whether the table has only 1 row.
If `singleRow` is set then each sub object can be referenced directly rather than through an array reference
(e.g. tablename.fiedl instead of tablename[0].field)

`join`: the join command specifies an array of Source Objects.

Example:
```
{
  "name": "exampleTable",
  "singleRow": false,
  "join": [
    <source object 1>,
    <source object 2>
  ]
}
```


### Join Source Objects

A Join operation requires at least 2 source tables.
  * The first source table must have a valid common value for every document.
  * The common key value will be taken from the first source table.
  * To be joined, subsequent tables must have a common key that matches the 1st source table common value.
  * The resultant table will have as many rows as the first source table.
  * If subsequent source tables (2nd table and beyond) do not have a matching common the fields will simply not be included.
  * Fields have priority from left to right.  In other words, if there are fields with the same name in the 1st source table and the 2nd source table,
the resultant key value in the destination table will have the value from the 1st source table.


Source data is specified as an array of objects with the following keys:

`table*`: Required: this is the name of the source table.

`common*`: Required: this is the common key specification for this table. This will equal the key name.  If there are more than 1 source tables a common is required.

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

