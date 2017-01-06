## JOIN operations

### Calculated data (destination data)

Join data is calculated from source data and placed into the current CSV file's named table. (e.g. the $VAR variable `name`).

* Any data in the destination will flag an error.
Destination table options can be specified via the `options` object.

`options`: The following options can be set on a calculated table:

* `singleRow`: `true|false`
* `onDemand`: `true|false`

### Source data

It is only required that there be 1 set of source data.  Supplying 1 source could create a new *"subset"* table.  Other than that it is not useful.

When 2 or more sets of source data are supplied the following rules apply:

* Each table must have a common key with all other source tables.
* The number of rows of the join will be equal to the number of rows in the first source table.
* The common key value will be taken from the first source table.
* Fields have priority from left to right.  In other words, if there are common join fields in the 1st source table and the 2nd source table, the resultant key value in the destination table will have the value from the 1st source table.

Keys can be filtered from a table via an `incl` list or an `excl` list.

Source data is specified as an array of objects with the following keys:

`table*`: this is the name of the source table.  This can equal the current table, see `dstTable` rules above.

`common*`: this is the common key specification for this table. This will equal the key name.

`incl`: specify a key name or an array of key names that you want to include in the join.  If the `incl` option is used only the keys specified in `incl` will be joined.

`excl`: sometimes it is simpler to specify the fields you want to exclude from the join.  Use `excl` to specify a key name or an array of key names that you want to exclude from the join.

`xlt`: this is an optional translation object.  Each object key is a translation specification of the form:
```
{"<src keyname>": "<new keyname>"}
```
In essence, this is a way to rename a field and put it into the calculated table.

`subObj`: sometimes it is useful to group all source fields into a sub-object.  This can be accomplished with the `subObj` parameter.  `subObj` can be used in conjunction with `xlt` to put translated `incl` fields into the `subObj`.

Notes:

* `*` fields are required.
* only one of `incl` or `excl` can be specified.  If both are specified an error will be thrown.
* For filter operations use mongo dot notation to refer to sub keys.  `key.subkey` specifies the subkey of the key object.


Example specification:

```
[{
    "table": "<name of 1st table>",
    "common": "<this tables common key>",
  }, {
    "table": "<name of 2nd table>",
    "common": "<this tables common key>",
    "incl": "oneKey"
  }, {
    "table": "<name of 3rd table>",
    "common": "<this tables common key>",
    "incl": ["_id", "name", "address"]
    "xlt": {"_custID", "custname", "src": "translation"}
  },
  ...
    "table": "<name of table n>",
    "common": "<this table's common key>",
    "excl": ["<key1 to exclude>", "<key2 to exclude>"]
    "subobj": "parts"
  }]
```
