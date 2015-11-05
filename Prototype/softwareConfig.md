# Software Configuration
## Receiving Computer
This guide assumes you're running a recent version of Ubuntu. The StereoscoPi prototype software is written in Python, and should have good cross-platform compatibility. Likewise GStreamer supports most major operating systems - it should be possible to replicate this setup on Windows or OSX. For the sake of simplicity, we will use Ubuntu here.
### GStreamer Installation
First we need to tell our system where to look for the GStreamer framework:
`sudo add-apt-repository ppa:gstreamer-developers/ppa`

Update:

`sudo apt-get update`

Install GStreamer 1.0:

`sudo apt-get install gstreamer1.0*`

Test GStreamer is installed:

`gst-inspect-1.0 fakesrc`

If succesful, you should see something similar to the following:

```
Factory Details:
Rank                     none (0)
Long-name                Fake Source
Klass                    Source
...
...
...
Element Signals:
"handoff" :  void user_function (GstElement* object,
                                 GstBuffer* arg0,
                                 GstPad* arg1,
                                 gpointer user_data);
```

### Download StereoscoPi Scripts
Download the files in the 'Scripts' folder. You can either do this manually, by clicking each file, and clicking on 'Raw' then saving, or we can clone the needed files with Git.

Because we don't want to clone the whole repository, there are a few more steps than usual. First, make an empty folder to store the files in:

```
mkdir StereoscoPi
cd StereoscoPi
```

Now let's initialise git and clone the directory we want:

```
git init
git remote add -f origin https://github.com/chrissbarr/StereoscoPi.git
git config core.sparseCheckout true
echo "Prototype/Scripts/" >> .git/info/sparse-checkout
git pull origin master
```

Change into the 'Scripts' directory, and check that you have the files:

`cd Prototype/Scripts/`
`ls`

You should see the files (or similar):

```receiveStream.py      transmitStream.sh```

