### New System TODOs
I just updated my system using a clean install.  This caused me some pain because
I failed to save out a few key pieces of information of my development environment.

#### [Install node-gyp](https://github.com/nodejs/node-gyp) first
Specifically run this first:
```
npm install --global --production windows-build-tools
```
### Match node architecture
In order for node-inspector to work it should be installed in the same version/architecture as the meteor node version.  Today meteor's node version is 32 bit.

Steps:
  1. Get the meteor node version: `meteor node -v`
  2. Find matching version [here](https://nodejs.org/en/download/releases/).
  3. Download the same architecture, today 32 bit.
  4. Install node.
  5. Then install gyp (see above).
  6. Then install node-inspector: `npm install node-inspector -g`
  7. Good to go!

#### Outlook: save out rules to an rwz file
I saved out my mailboxes and I depend on my hotmail accounts anyway.  But I failed to save out my rules.  Luckily I had an old
version laying around which had most of my rules.

#### Recreating the Mongo DB is a little tricky
I updated my [instruction](https://github.com/brucejo75/Helpful-docs/blob/master/Setup%20Independent%20Mongo%20Process.md) file with
a few details that were missing.

#### Sublime edit
Install:
* Package Control - Package manager
* SublimeLinter - This is the lint manager.  Install instructions [here](http://* sublimelinter.readthedocs.io/en/latest/installation.html).
* SublimeLinter-contrib-eslint - This is the eslint package and needs to be special installed.  Instructions [here](https://github.com/roadhump/SublimeLinter-eslint).
* emmet - emmet does some html and css autocompletion.  Simple install.  Use instructions [here](https://github.com/sergeche/emmet-sublime#tab-key-handler).

##### Disable Intel hotkeys
Sublime uses `Ctrl-Alt-down` for multi line editing.  They are used by the intel video driver.  Disable them with the tray app.
