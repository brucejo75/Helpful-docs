# Windows: Configure mongo server to accept connections from remote clients

### [Open up port for external access](https://docs.mongodb.com/manual/tutorial/configure-windows-netsh-firewall/#traffic-to-and-from-mongod-exe-instances)

This pattern is applicable to all mongod.exe instances running as standalone instances or as part of a replica set. The goal of
this pattern is to explicitly allow traffic to the mongod.exe instance from the application server.

`netsh advfirewall firewall add rule name="Open mongod port 27017" dir=in action=allow protocol=TCP localport=27017`

This rule allows all incoming traffic to port 27017, which allows the application server to connect to the mongod.exe instance.

### Bind mongo server to IP addresses

Change mongo config file:

```
$ vim /etc/mongod.conf

# /etc/mongod.conf

# Listen to local and LAN interfaces.
bind_ip = 127.0.0.1, 192.168.161.100
```
where `192.168.161.100` is the server **NOT** the client.  What this says is that connections addressed to either 127.0.0.1 or 192.168.161.100
will be able to connect to the mongo server.

### [Create a windows Service](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#manually-create-a-windows-service-for-mongodb-community-edition)
Follow the directions and you will have a mongo DB that runs as a service.  Then each of your Meteor apps can connect to it.
