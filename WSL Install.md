# Instructions for setting up Windows Subsystem For Linux

## rc files
There are a number of shell initialization files:
- `/etc/bash.bashrc` - default bash setup stuff
- `~/.bashrc` - default bash setup stuff.  `~/.bashrc` sources rc file at `/mnt/e/u/bruce/.bashrc`
This file is easier to edit because it is on NTFS.
- `.merisrc` - Root environment variables for MERIS setup.
- `ROOT/app/.merisrc` & `$MROOT/meris/.merisrc` - contains the settings for the meris build and the app build.

## BASH Shell shortcuts
### Meris
- Target: `C:\Windows\System32\bash.exe --rcfile /mnt/e/u/mroot/app/.merisrc`
- Start In: `e:\u\mroot\app`

### App
- Target: `C:\Windows\System32\bash.exe --rcfile /mnt/e/u/mroot/meris/.merisrc`
- Start In: `e:\u\mroot\meris`

## Install Node & npm
### PreRequisites
#### Java
[Install Java Ubuntu](https://poweruphosting.com/blog/install-java-ubuntu/)

- `sudo apt-get install`
- `java -version` ?  If not, continue
- `sudo apt-get install default-jre`
- `java -version` ?  Should work now
#### nvm
[Using NVM](http://nodesource.com/blog/installing-node-js-tutorial-using-nvm-on-mac-os-x-and-ubuntu/)

- Check latest nvm version [here](https://github.com/creationix/nvm/releases)
- Latest at pub: `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash`
- Latest at pub: `curl -o- https://raw.githubusercontent.com/creationix/nvm/<version>/install.sh | bash`

### Install Node
[Installing Node.js using nvm](http://nodesource.com/blog/installing-node-js-tutorial-using-nvm-on-mac-os-x-and-ubuntu/)

`nvm install node 8.8.1`

## Install Meteor
### Prerequisites
#### Python2.7
[Install Python2.7 on Ubuntu](https://tecadmin.net/install-python-2-7-on-ubuntu-and-linuxmint/)

- `sudo apt-get update`
- `sudo apt-get install build-essential checkinstall`
- `sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev`
- `cd /usr/src`
- Check latest Python version [here](https://www.python.org)
- `sudo wget https://www.python.org/ftp/python/2.7.14/Python-2.7.14.tgz`
- `sudo tar xzf Python-2.7.13.tgz`
- `cd Python-2.7.14`
- `sudo ./configure`
- `sudo make altinstall`
- Check version: `python2.7 -V`
- Create link to python in `/usr/loca/bin`: `sudo ln -s python2.7 python`

#### make
- Make sure make is installed: `make --version`

#### node-gyp
[Installation](https://github.com/nodejs/node-gyp#installation)

`npm install -g node-gyp`

### Meteor
[Installation](https://www.meteor.com/install)

`curl https://install.meteor.com/ | sh`
