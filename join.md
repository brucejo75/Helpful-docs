## JOIN operations

### Calculated data (destination data)

The calculated join data is copied into a destination table.

`dstTable`: **destination table**, this is the table that will contain the result of the join.

* The `dstTable` can equal the name of the current table.  If it is equal to the current table then the join will calculate *in-place* with the following restrictions:
  * The current table is **required** to be the first source table.
  * All fields from the current table will be joined and are unable to be overwritten by fields joined from subsequent source tables.
  * A 2nd source table is **required** when the destination table is the current table.
* If `dstTable` is equal to the name of an existing table, then an error will be raised and the join will not be performed.
* If `dstTable` is a unique name.
  - If the current table is empty then sources will specify the tables to join.
  - If the current table has data, then the current table can **optionally** be specified as a source for the join with no restrictions.

Destination table options can be specified via the `options` object.

`options`: The following options can be set on the calculated table:

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

`incl`: specify a key name or an array of key names that you want to include in the join.

`excl`: sometimes it is simpler to specify the fields you want to exclude from the join.  Use `excl` to specify a key name or an array of key names that you want to exclude from the join.

Notes:

* only one of `incl` or `excl` can be specified.  If both are specified an error will be thrown.
* For filter operations use mongo dot notation to refer to sub keys.  `key.subkey` specifies the subkey of the key object.

Example specification:

```
{
  dstTable: "<name of dest table>",
  options: {
    singleRow: true,
    onDemand: true
  },
  sources:[{
    table: "<name of 1st table>",
    common: "<this tables common key>",
  }, {
    table: "<name of 2nd table>",
    common: "<this tables common key>",
    incl: "oneKey"
  },{
  ...
    table: "<name of table n>",
    common: "<this table's common key>",
    excl: ["<key1 to exclude>", "<key2 to exclude>"]
  }]
}
```
