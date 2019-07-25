## How to update Mac tools
Instructions for how to update all the tools on the MacOS Server
### Tool List
1. nvm
2. node (nvm)
3. npm (nvm)
4. pm2 (nvm)
5. bunyan (nvm)
6. minio (brew service)
7. mongodb (brew service)
8. caddy (brew service)
10. mc (brew)

### Process Flow
1. install nvm
2. update update node
3. install pm2 & bunyan -> nvm:node
4. brew update minio, mongodb & caddy

### nvm
get latest command at [nvm-sh/nvm](https://github.com/nvm-sh/nvm#installation-and-update).

for v0.34.0:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
```

### node
[Using nvm](https://github.com/nvm-sh/nvm#usage):

```bash
nvm install <version #>
e.g.
nvm install 8.15.1
```
Will need to restart the shell.

### npm
```bash
npm install -g npm@<version #>
e.g.
npm install -g npm@6.9.0
```
Will need to restart the shell.

### pm2
[Update pm2](http://pm2.keymetrics.io/docs/usage/update-pm2/):

```bash
pm2 save
npm install pm2 -g
pm2 update
```
Will need to restart the shell.

### bunyan
```bash
npm install -g bunyan
```
Will need to restart the shell.

## Brew Services
Automatically, sets up OSX services that will be started at reboot.  To get help enter `brew services`

```bash
brew install minio
brew install mongodb@3.4
brew install caddy
brew install minio/stable/mc
```
- plists can be found at `mroot/scripts/plists`
- plists are installed to `/user/local/Cellar`

```bash
cd /usr/local/Cellar
```

dig into each recipe and then copy in the plist from `mroot/scripts`

brew will copy these plists to `/Library/LaunchDaemons/`

### Update all
[FAQ](https://docs.brew.sh/FAQ)

Update brew, then upgrade all services:
```bash
brew update
brew outdated
brew upgrade
```
## MinIO client (mc) config
Configure mc for easy usage with meris.  [Add a cloud storage service](https://docs.min.io/docs/minio-client-complete-guide.html) alias:

```bash
mc config host add <ALIAS> <YOUR-S3-ENDPOINT> <YOUR-ACCESS-KEY> <YOUR-SECRET-KEY> <API-SIGNATURE>
mc config host add meris <meris s3 url> <ACCESS-KEY> <SECRET-KEY>
```
