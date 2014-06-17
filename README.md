PocketSphinx
=====================

This repository contains all the prerequisites properly configured in order to use PocketSphinx.  We have installed both *SphinxBase*, as well as *PocketSphinx* as suggested.  This repository can be forked, then used as a submodule.

##PocketSphinx

###Definition:

PocketSphinx is a lightweight speech recognition engine, specifically tuned for handheld and mobile devices, though it works equally well on the desktop.

- https://github.com/cmusphinx/pocketsphinx

###Overview:

Though, this can be forked to any project - we are specifically going to use it as a submodule in our other project - https://github.com/jeff1evesque/audio-analyzer

##Requirement

###Pre-Installation:

```
sudo apt-get update
sudo apt-get install libtool
sudo apt-get install autoconf
sudo apt-get install bison
sudo apt-get install swig2.0
```

###Configuration

####GIT:

Fork this project on your GitHub account, then clone it:

```
cd /var/www
sudo git clone https://jeff1evesque@github.com/[YOUR-USERSNAME]/pocketsphinx-custom.git pocketsphinx-custom
```

Then, add git upstream reference:

```
cd /var/www/pocketsphinx-custom
git remote add upstream https://github.com/jeff1evesque/pocketsphinx-custom.git
```

Initialize any submodules we are using:

```
cd /var/www/pocketsphinx-custom
git submodule init
git submodule update
```

####File Permission:

Change the file permission for the entire project by issuing the command:

```
cd /var/www
sudo chown -R jeffrey:admin pocketsphinx-custom
```

**Note:** change *jeffrey* to YOUR username.

###Installation, and Updates:

SphinxBase is a dependency for *PocketSphinx*.  Therefore, we need to incorporate both submodules.

- https://github.com/cmusphinx/pocketsphinx/blob/master/README

####SphinxBase:

```
cd /var/www/pocketsphinx-custom/sphinxbase
git checkout -b [NEW_BRANCH] MASTER
git add [FILE]
git commit -m "#i: MESSAGE"

cd ..
git add sphinxbase
git commit -m "#i: MESSAGE"
git push origin [NEW_BRANCH]
```

Then, submit a pull-request, and merge the above changes.

####PocketSphinx:

Repeat the latter steps tailored for PocetSphinx.
