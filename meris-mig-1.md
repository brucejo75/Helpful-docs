# Meris Migration 1
This is a cleanup migration of the DB and the servers

Pull all execution data under the `MERIS_LIVE_HOME` directory.
- gather the logs into a single root
- gather all data into a single root
- gather all configuration files into a single root

## Shutdown steps
1. `pm2 kill`
2. `brew services stop mongodb@3.4`

**Important:** verify that all processes are killed via `ps aux`.

## Move Mongo data
1. `mv ~/MERISDB/MERIS/* ~/MERIS/mongo/db`
2. `mv ~/MERISDB/MERISLOG/* ~/MERIS/mongo/logs`
3. `cp ~/mroot/conf/mongo/homebrew.mxcl.mongodb@3.4.plist ~/MERIS/mongo`
3. `cp ~/MERIS/mongo/homebrew.mxcl.mongodb@3.4.plist /usr/local/Cellar/mongodb\@3.4/3.4.18/`

## Restart Services
1. `brew services start mongodb@3.4`
2. `nr meris:deploy root` (version 0.2.5)

## Verify logs are active
1. `~/MERIS/mongo/logs`
2. `~/MERIS/servers/logs`

## Enable logrotate
1. `cp homebrew.mxcl.logrotate.plist /usr/local/Cellar/logrotate/3.15.0/`
2. `brew services start logrotate`
