### Upgrade to v1.4

#### Mongo 3.2 DB changes
- Upgrade to [SCRAM-SHA-1](https://docs.mongodb.com/manual/release-notes/3.0-scram/#upgrade-mongodb-cr-to-scram), essentially: `use admin, db.adminCommand({authSchemaUpgrade: 1});`
- Maybe delete and re-add the oplog reader user

#### Make sure `node-gyp` is installed
- [Install `node-gyp`](https://github.com/nodejs/node-gyp#installation)

#### Clear out old stuff
- `rm -rf .meteor/local`
- [Rebuild binaries](https://guide.meteor.com/1.4-migration.html#binary-packages-require-build-toolchain), essentially: `meteor npm rebuild`

#### Make sure no `debugger` statements in code

#### Ran into this [`npm-bcrypt`](http://stackoverflow.com/questions/29340510/bcrypt-is-breaking-my-meteor-application-how-do-i-fix-it/29347616) problem
