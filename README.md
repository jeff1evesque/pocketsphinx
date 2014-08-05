PocketSphinx
=====================

This repository contains all the prerequisites properly configured in order to use PocketSphinx.  We have installed both *SphinxBase*, as well as *PocketSphinx* as suggested.  This repository can be forked (or directly submoduled), then used as a submodule.

For questions regarding this project, or implementations of it on other projects, feel free to reach out:

Jeffrey Levesque  
jeff1evesque@yahoo.com

###Definition

[PocketSphinx](https://github.com/cmusphinx/pocketsphinx) is a lightweight speech recognition engine, specifically tuned for handheld and mobile devices, though it works equally well on the desktop.

##Pre-Installation

###Linux Packages

```
# General Packages:
sudo apt-get update
sudo apt-get install libtool
sudo apt-get install autoconf
sudo apt-get install bison
sudo apt-get install swig2.0
```

**Note:** This project assumes [Ubuntu Server 14.04](http://www.ubuntu.com/download/server) as the operating system.

##Configuration

###GIT

Fork this project on your GitHub account, then clone it:

```
cd /var/www/html/
sudo git clone https://jeff1evesque@github.com/[YOUR-USERSNAME]/pocketsphinx.git pocketsphinx
```

Then, add git upstream reference:

```
cd /var/www/html/pocketsphinx/
git remote add upstream https://github.com/jeff1evesque/pocketsphinx.git
```

Initialize any submodules we are using:

```
cd /var/www/html/pocketsphinx/
git submodule init
git submodule update
```

###File Permission

Change the file permission for the entire project by issuing the command:

```
cd /var/www/html/
sudo chown -R jeffrey:admin pocketsphinx
```

**Note:** change *jeffrey* to YOUR username.

##Installation /  Updates

Since SphinxBase is a [dependency](https://github.com/cmusphinx/pocketsphinx/blob/master/README) for PocketSphinx, both submodules are required.

###SphinxBase

```
cd /var/www/html/pocketsphinx/sphinxbase/
git checkout master
git pull
./autogen.sh
sudo make install

cd ../
git add sphinxbase
git commit -m "#i: MESSAGE"
git push origin [NEW_BRANCH]
```

Then, submit a pull-request, and merge the above changes.

###PocketSphinx

Repeat the latter steps tailored for the PocketSphinx submodule.

###SphinxTrain

Repeat the latter steps tailored for the SphinxTrain submodule.

###Acoustic / Language Models

To improve the accuracy of audio translations, we will utilize two additional models.  An *Acoustic*, and *Language* model:

```
# Extract Acoustic, and Language Models
cd ../../
wget http://sourceforge.net/projects/cmusphinx/files/Acoustic%20and%20Language%20Models/US%20English%20Generic%20Acoustic%20Model/en-us.tar.gz/download -O en-us.tar.gz
sudo tar -zxvf en-us.tar.gz -C /usr/local/share/pocketsphinx/model/hmm/
sudo rm en-us.tar.gz

wget http://sourceforge.net/projects/cmusphinx/files/Acoustic%20and%20Language%20Models/US%20English%20Generic%20Language%20Model/cmusphinx-5.0-en-us.lm.dmp/download -O cmusphinx-5.0-en-us.lm.dmp
sudo mv cmusphinx-5.0-en-us.lm.dmp /usr/local/share/pocketsphinx/model/lm/
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

###Translation Time

The [PocketSphinx](http://cmusphinx.sourceforge.net/wiki/tutorialpocketsphinx) translation engine ideally should have a *translation time* **(TR)** equal to three times the *recording time* **(RT)**:

```
TR = 3 x RT
```

The *translation time* (TR) can be verified by checking the output from the command `pocketsphinx_continuous`.  This command will output many lines.  However, the ones of particular relevance have a very specific form.

###CPU Time

The *CPU Time* is the actual *execution time* for the `pocketsphinx_continuous` command.  Therefore, the sum of all such lines will produce the overall CPU Time for the `pocketsphinx_continuous` command:

```
ngram_search_fwdtree.c(xxx): TOTAL fwdxxxx xx.xx CPU x.xxx xRTINFO:
``` 

###System Time

The *Wall Time* is the actual *system time* for the `pocketsphinx_continuous` command.  A system can pause processes for various operations, including those used in relation to `pocketsphinx_continuous`.  Therefore, possibly a better measure of the overall translation time.  The sum of all such lines will produce the overall *System Time* for the `pocketsphinx_continuous` command:

```
ngram_search_fwdtree.c(xxx): TOTAL fwdtxxxx xx.xx wall x.xxx
```

###Translation Accuracy

Depending on whether additional *Acoustic*, or *Lanuguage* models are used, the translation accuracy can be significantly influenced.  Generally, the *Acoustic model* describes the sounds of the language, whereas the *Language model* describes the probability of word sequences.  In order to create our own acoustic model, we need to utilize the *SphinxTrain* submodule in this repository.  To create our own language model, we need to use *srilm*, a non-free licensed tool.

One thing to keep in mind, the *Sphinx* engine has difficulty translating *language fillers*, words generally used to express pauses (uh, hmm, err, ahh, etc.) within speech.  Language fillers, at times may be ignored, translated into other known words, or distorting the translation of words spoken within close (immediate) proximity of the language filler.
