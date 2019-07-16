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

### Process Flow
1. install nvm
2. update update node
3. install pm2 & bunyan -> nvm:node
4. brew update minio, mongodb & caddy

### nvm
get latest command at [nvm-sh/nvm](https://github.com/nvm-sh/nvm#installation-and-update).

for v34:
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

### npm
```bash
npm install -g npm@<version #>
e.g.
npm install -g npm@6.9.0
```

### pm2
[Update pm2](http://pm2.keymetrics.io/docs/usage/update-pm2/):

```bash
pm2 save
npm install pm2 -g
pm2 update
```

### bunyan
```bash
npm install -g bunyan
```

## Brew Services
Automatically, sets up OSX services that will be started at reboot.  To get help enter `brew services`

```bash
brew install minio
brew install mongodb@3.4
brew install caddy
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


