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

### Configuring receiveStream.py
The receiveStream.py script requires the following information:

* Screen Resolution (i.e. 1920x1080 for the Oculus DK2)
* Pi 1 Port (i.e. 5000)
* Pi 2 Port (i.e. 5001)

Edit the receiveStream.py file, and add these details in the 'Required Configuration' section near the top of the file, as shown:

```
# Required Configuration
display_width = 1920
display_height = 1080
port1 = '5001'
port2 = '5000'
```

## Transmitting Computer
You'll need to repeat this process for each Pi. First you'll need to configure each Pi for SSH access - Adafruit have a great tutorial for doing this [here](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-6-using-ssh). Once you have SSH access set up, continue with the steps below.

### Install GStreamer
`sudo nano /etc/apt/sources.list`

Add the following line to the end of the file:

`deb http://vontaene.de/raspbian-updates/ . main`

CTRL+X, save the file over the original, and exit. Update:

`sudo apt-get update`

Download GStreamer:

`sudo apt-get install gstreamer1.0`

Just as we did on the receiving computer, test GStreamer is installed:

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

### Copy transmitStream.sh
When we downloaded the scripts to the receiving computer earlier, we downloaded the script we need to place on the Pi - transmitStream.sh. Assuming you're still in the 'Prototype/Scripts/' folder on the receiving computer, copy the file over SSH as follows:

`scp transmitStream.sh user@remote.host:/scripts`

On the Pi, check that the file is present. Alternatively, you can repeat the git process we used above, or copy the file across any other way you'd like.

## Running Software
### Running transmitStream.sh on each
First we need to run transmitStream.sh on each Pi. transmitStream.sh is really just a more convenient way of typing a rather long GStreamer command - namely:

`raspivid -t 999999 -w $WIDTH -h $HEIGHT -fps $FRAMERATE -rot 0 -b $BITRATE -n -pf baseline -md 5 -o - | gst-launch-1.0 -e -vvv fdsrc ! h264parse ! rtph264pay pt=96 config-interval=1 ! udpsink host=$TARGET_IP port=$PORT`

You can run the transmitStream.sh script as follows:

`bash transmitStream.sh 192.168.2.4 5000 1280 720 50 3000000`

Where the arguments represent the following:
* 192.168.2.4 - IP Address of the computer that will run receiveStream.py
* 5000 - Port that we want to transmit on. This needs to be different for each Pi - 5000 and 5001 are good numbers.
* 1280 and 720 - width and height, respectively, of video acquired from the attached camera
* 50 - video framerate
* 3000000 - bitrate the video is encoded at. Dependent on network conditions, decrease if video is jerky / high-latency, increase for higher quality.

When the script is running, the LED on the camera module should light up, indicating the camera is active.

