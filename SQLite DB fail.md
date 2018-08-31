# Repair broken SQLITE packages DB

Error message: `SQLITE_CORRUPT: database disk image is malformed` comes up occasionally.

Need to dump the DB and then restore it.

```
cd ~/.meteor/package-metadata/v2.0.1
sqlite3 packages.data.db .dump > backup
mv packages.data.db packages.data.db.bustedX
sqlite3 packages.data.db < backup
rm backup
```
