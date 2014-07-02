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
sudo git clone https://jeff1evesque@github.com/[YOUR-USERSNAME]/pocketsphinx.git pocketsphinx
```

Then, add git upstream reference:

```
cd /var/www/pocketsphinx
git remote add upstream https://github.com/jeff1evesque/pocketsphinx.git
```

Initialize any submodules we are using:

```
cd /var/www/pocketsphinx
git submodule init
git submodule update
```

####File Permission:

Change the file permission for the entire project by issuing the command:

```
cd /var/www
sudo chown -R jeffrey:admin pocketsphinx
```

**Note:** change *jeffrey* to YOUR username.

####Local Ignore Rules

We do not want to commit files, or directories within our git *submodules*.  For this reason, we need to add git *local ignore rules*.  This is done by changing into the directory of the submodule and editing the following file:

```
cd /var/www/pocketsphinx/[YOUR_SUBMODULE]
pico .git/info/exclude
```

Then, add the following, and save the file:

```
*
```

**Note:** each repository (or submodule) has it's own `.git/info/exclude` file. 

###Installation, and Updates:

SphinxBase is a dependency for *PocketSphinx*.  Therefore, we need to incorporate both submodules.

- https://github.com/cmusphinx/pocketsphinx/blob/master/README

####SphinxBase:

```
cd /var/www/pocketsphinx/sphinxbase
git checkout master
git pull
./autogen.sh
sudo make install

cd ..
git add sphinxbase
git commit -m "#i: MESSAGE"
git push origin [NEW_BRANCH]
```

Then, submit a pull-request, and merge the above changes.

####PocketSphinx:

Repeat the latter steps tailored for the PocketSphinx submodule.

####Additional PocketSphinx Models

To improve the accuracy of audio translations, we will utilize two additional models.  An *Acoustic*, and *Language* model.  These can be downloaded from the following sources, respectively:

- http://sourceforge.net/projects/cmusphinx/files/Acoustic%20and%20Language%20Models/US%20English%20Generic%20Acoustic%20Model/

- http://sourceforge.net/projects/cmusphinx/files/Acoustic%20and%20Language%20Models/US%20English%20Generic%20Language%20Model/

Then, we need to ensure `en-us` is stored in the following directory:

```
/usr/local/share/pocketsphinx/model/hmm/en-us
```

and, `cmusphinx-5.0-en-us.lm.dmp` (extracted *language model*) is stored in the following directory:

```
/usr/local/share/pocketsphinx/model/lm/cmusphinx-5.0-en-us.lm.dmp
```

##Testing / Execution

When everything has been installed, and configured, *PocketSphinx* is very easy to run.  For example, we can run the following commands:

```
# converts SAMPLE.wav to text within the terminal console
pocketsphinx_continuous -infile SAMPLE.wav

# converts SAMPLE.wav to text while using additional Acoustic, and Language models respectively
pocketsphinx_continuous -infile SAMPLE.wav -hmm en-us -lm en-us.lm.dmp

# converts SAMPLE.wav, and redirects output to sample.txt
pocketsphinx_continuous -infile SAMPLE.wav > sample.txt
```

###Flags

`hmm en-us`: loads *Acoustic model* from the `en-us` folder

`lm en-us.lm.dmp`: loads *Language model* from the file `en-us.lm.dmp`

###Output

The resulting text translation will look like the following, whether redirected to a text file, or stdout within the terminal console:

```
000000000: this is a test conversion result using this applcation
000000001: the program processes audio by splitting on the audio into chunks
000000002: the chunks are determined by silence fragments within the audio input file
```

###Translation Time (TR)

The trasnlation time (TR) suggested by a CMUSphinx developer, should be three times the recording time (RT):

```
TR = 3xRT
```

###Translation Accuracy

Depending on whether additional *Acoustic*, or *Lanuguage* models are used, the translation accuracy can be significantly influenced.  One thing to keep in mind, the *Sphinx* engine has difficulty translating *language fillers*, words generally used to express pauses (uh, hmm, err, ahh, etc.) within speech.  Language fillers, at times may be ignored, or translated into some other known word.
