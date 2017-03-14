## Field modification operators

`incl`: specify a key name or an array of key names that you want to include in the join.
If the `incl` option is used only the keys specified in `incl` will be joined.

-or-

`excl`: sometimes it is simpler to specify the fields you want to exclude from the join.
Use `excl` to specify a key name or an array of key names that you want to exclude from the join.

The `incl` operator and `excl` operator are mutually exclusive.

`xlt`: this is an array of translation objects.  Each object contains 2 keys, "old" and "new":
```
{"old": "<src keyname>" "new": "<new keyname>"}
```
In essence, this is a way to rename a field and put it into the calculated table.

`subObj`: sometimes it is useful to group all source fields into a sub-object.  This can be accomplished with the `subObj` parameter.
`subObj` can be used in conjunction with `xlt` to put translated `incl` fields into the `subObj`.

Notes:

* `*` fields are required.
* only one of `incl` or `excl` can be specified.  If both are specified an error will be thrown.
* For all operations you can use mongo dot notation to refer to sub keys.  `key.subkey` specifies the subkey of the key object.


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
    "xlt": [{"old": "_custID", "new": "custname"}, {"old": "src", "new": "translation"}]
  },
  ...
    "table": "<name of table n>",
    "common": "<this table's common key>",
    "excl": ["<key1 to exclude>", "<key2 to exclude>"]
    "subobj": "parts"
  }]
```
