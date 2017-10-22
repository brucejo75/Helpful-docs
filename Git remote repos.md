# Git commands to create a new server repo from a subdir
### Create clone of your project in .git file

```
$ cd my_project
my_project$ cd ..
$ git clone --bare my_project my_project.git
$ copy my_project.git -> server
$ delete my_project
$ git clone file:////mpc/git/my_project.git
$ git remote -v
origin  file:////mpc/git/my_project.git (fetch)
origin  file:////mpc/git/my_project.git (push)
```

Now new my_project directory is a clone of your server .git project.

### Carve off sub from my_project
Taken from [here](http://stackoverflow.com/questions/359424/detachmove-subdirectory-into-separate-git-repository/17864475#17864475).

Working on this tree:

```
my_project
├── sub
```

Want to carve sub directory out of subtree.

A. Create branch `sub-only` that only has git commands for `sub` directory.
```
my_project$ git subtree split -P sub -b sub-only
Created branch 'sub-only'
ab1f70045edfab1d8b4190ce800d24ee17cf855d
```
B. Create new location for new `sub` project & pull the code into it from the local disk.
```
my_project$ cd ..
$ md sub
$ cd sub
$ git init
$ git pull /my_project sub-only
```
C. Create clone of the project and copy to server.
```
 $ cd sub
 sub$ cd ..
 $ git clone --bare sub sub.git
 $ copy sub.git -> server
```
 D. Delete project from disk and the clone from the remote server.
```
 $ delete sub
 $ git clone file:////mpc/git/sub.git
 $ cd sub
 sub$ git remote -v
 origin  file:////mpc/git/sub.git (fetch)
 origin  file:////mpc/git/sub.git (push)
```
### Add sub-module
Taken from [here](https://chrisjean.com/git-submodules-adding-using-removing-and-updating/).

Now that we have separated out the sub-module as it's own project we now want to put it back into the tree.

A. Clear the sub project from the my_project tree
```
$ cd my_project
my_project$ del sub
my_project$ git add *
my_project$ git commit
```
B. Put the sub module into the tree
```
$ cd my_project
my_project$ git submodule add file:////mpc/git/sub.git sub
git add sub
git commit
```
C. Initialize and prep the submodule
```
my_project$ git submodule init
my_project$ git submodule update

```
### Annotated Tag
```
git tag -a <tag> -m "<message>"

e.g.:
git tag -a v1.1 -m "Version 1.1"
```
