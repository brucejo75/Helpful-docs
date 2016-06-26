## Set up Independent Mongo process for Meteor Development

While developing my Meteor application I have started to require running my Mongo process separate from the process that Meteor fires up when it is run.

There are a couple of steps to setting this up:
  1. My Mongo config options - this will tell give you a set of config options that are necessary for setting up this DB.
  2. Start the database.
  3. Initialize replica set.  - to get the replica set working you need to do an [`rs.initiate()`](https://docs.mongodb.com/manual/reference/method/rs.initiate/).
  4. Intialize the admin DB and your DB.
  5. Set up an oplog reader user in your mongo DB.
  6. Set up environment variables and run meteor.


###1.  My Mongo [config options](https://docs.mongodb.com/manual/reference/configuration-options/)

I am finding that using [mongo configuration files](https://docs.mongodb.com/manual/reference/configuration-options/#configuration-file) is really the way to go for me.  So here is my example config file and I will describe what each section means:

>A quick warning about mongo config files, they use YAML as their file type.  YAML is an indentation standard and it can have trouble with tabs v. spaces and I was unsure if/where quoting was used.  This [YAML linter](http://www.yamllint.com/) is a good tool to use to make sure your file is formatted correctly.

On Windows:
```
systemLog:
    destination: file
    path: c:\DB\DBNAME_LOG\DBNAME.log
storage:
    dbPath: c:\DB\DBNAME
net:
    bindIp: DBHOST
    port: 27017
replication:
   replSetName: rs0
   oplogSizeMB: 8

```
#### [systemLog](https://docs.mongodb.com/manual/reference/configuration-options/#systemlog-options):

This is pretty simple it specifies that I am logging to a file and if I log to a file I also am required to specify a path.

#### [storage](https://docs.mongodb.com/manual/reference/configuration-options/#storage-options):

Here I am simply specifying the path to where the DB is on disk.

####[net](https://docs.mongodb.com/manual/reference/configuration-options/#net-options):

This one is important and valuable.  If you want your mongo DB to be able to connect using your LAN you are required to enter the [bindIP](https://docs.mongodb.com/manual/reference/configuration-options/#net.bindIp) option correctly.  This address must be accessible from any client server that accesses the DB. I specify the LAN address of the computer running Mongo ( `DBHOST`).  Each of my server host machines have access to `DBHOST`.  Now other computers on my LAN can access this DB.  That way I can develop my Meteor app on one machine and host my mongoDB on another machine.

The [port](https://docs.mongodb.com/manual/reference/configuration-options/#net.port) is simply the port to connect to Mongo. I use the default `27017`.

#### [replication](https://docs.mongodb.com/manual/reference/configuration-options/#replication-options):

Meteor [supports reading the Mongo OpLog for observe operations](https://github.com/meteor/docs/blob/version-NEXT/long-form/oplog-observe-driver.md).  This provides a much more cpu efficient and is way faster than polling.

There are a number of "how tos" out there, but none that specifically focus on setting up a "single" replica.  They all seem to set up a replica set of at least 3 processes to initialize the opLog.  It is much simpler to set up a single replica.

#### [replSetName](https://docs.mongodb.com/manual/reference/configuration-options/#replication.replSetName):
This is the only option that is required to set up the replica set.  You just have to name it.

#### [oplogSizeMB](https://docs.mongodb.com/manual/reference/configuration-options/#replication.oplogSizeMB):

I was groveling through the [meteor code](https://github.com/meteor/meteor/blob/dce2b20ddbe45da48c37f26813fce8d0c72d8c88/tools/runners/run-mongo.js#L54) and found that they set up an `oplogSizeMB` of 8 MB for development so I followed their lead.

### 2. Start the database

##### Host MongoDB on Windows

**Optional**: If you want to access this DB from a separate computer you will need to open up the firewall.  See instructions [here](https://docs.mongodb.com/manual/tutorial/configure-windows-netsh-firewall/#traffic-to-and-from-mongod-exe-instances).

You want to set up a Windows service.  I followed the instructions [here](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#manually-create-a-windows-service-for-mongodb-community-edition).  Note: if you want to change config settings you just need to change the config file, stop the service and start it again.  There is a restart option on the service in the Task Manager -> Services tab.

##### [Host MongoDB on OSX](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)
On Lunix based machines you need only kick off `mongod` with the `--fork` flag.  Like this:
```
mongod --fork --config DB/db.cfg
```
If you want to change config settings on an OSX machine you will need to kill this processes and restart it.

### 3. Initialize the Replica set

Connect to your database (you can use [mongo shell](https://docs.mongodb.com/manual/mongo/), I have been using [robomongo](https://robomongo.org/)).  Run the [`rs.initiate()`](https://docs.mongodb.com/manual/reference/method/rs.initiate/) command:

```
rs.initiate({
  "_id" : "rs0",
  "members" : [ 
      {
          "host" : "DBHOST:27017",
      }
  ]
})
```
This sets 2 key variables:
#### The replica set name: `rs0`
#### The host for the primary member: `DBHOST`

It is important that these 2 variables are set properly, you can confirm their settings with a `rs.conf()` command.

That's it!  Now you should be able to examine collections on the DB.

### 4. Intialize the admin DB and your DB
To properly set up a user on your DB, you will need to add the user to the `admin` DB.  But first the admin DB has to be initialized.  Execute these mongo shell commands:

```
use admin  # Sets your current DB to admin
db.foo.insert({foo: "foo"}) # This will insert a collection named foo with a document
                            # that has one property foo set to foo
use DBNAME  # Set your current DB to DBNAME (substitute your DB name)
db.foo.insert({foo: "foo"}) # insert a dummy collection
db.show   # In addition to local you should now see admin and DBNAME databases
```
All they do is toss some data into the DB so that the DB initializes.  You can remove the foo collections once the DBs are operational with some real data.

### 5. Set up an oplog reader user on your admin DB

In order for Meteor to utilize the oplogs it needs to have access to the logs.  While connected to your database you can grant access by creating a user in the DB that has oplog read access using [`db.createUser`](https://docs.mongodb.com/manual/reference/method/db.createUser/):

```
use admin
db.createUser({ user: "logreader",
  pwd: "password",
  roles: ["read"]
})
```

Make sure you add the user to the admin DB.  Then create the user.

#### 6. Set up Environment variables

Finally, we can set up the environment variables required by Meteor:
```
MONGO_URL=mongodb://192.168.1.100:27017/DBNAME?replicaSet=rs0&connectTimeoutMS=60000&socketTimeoutMS=60000
MONGO_OPLOG_URL=mongodb://logreader:password@192.168.1.100:27017/local?authSource=admin
```
-or of you are running Mongo on the same machine as your Meteor process:
```
MONGO_URL=mongodb://127.0.0.1:27017/DBNAME?replicaSet=rs0&connectTimeoutMS=60000&socketTimeoutMS=60000
MONGO_OPLOG_URL=mongodb://logreader:password@127.0.0.1:27017/local?authSource=admin
```
The MONGO_URL environment variable contains some arguments:
- `replicaSet=rs0`: this replica set name matches the one set on rs.initiate().
- `connectionTimeoutMS=60000`: Healthy 60 second connection timeout
- `socketTimeoutMS=60000`: Healthy 60 second socket timeout

The MONGO_OPLOG_URL sets up the authenticated oplog reader URL.

Set these environment variables in your shell and you can run meteor and your database will be running where you specified.
