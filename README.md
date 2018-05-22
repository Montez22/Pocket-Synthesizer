# Pocket-Synthesizer
Helpful Documentational set up guide to a Raspberry Pi Pocket Synthesizer
Raspberry Pi Pocket Synthesizer
By Montez Hardy

This guide has been created to help anyone who does not or may have any experience with music synthesis relating to a Raspberry Pi 3 Model B.  This is my specific setup and path to setting up your own RPI Pocket Synthesizer. Sources that I have used for those who would like to dive in deeper will be posted below in the bibliography.
Appliances Needed:
•	Raspberry Pi 3 Model B (power source as well)
•	Midi Keyboard Device
•	UCTRONICS 3.5 Inch TFT LCD Display SPI with Touch Screen, Touch Pen for Raspberry Pi 3 Mode B
•	16 Gigabyte Micro SD card
•	Computer monitor (or a display with HDMI capabilities)
•	Reliable Internet connection (WIFI or Ethernet)
•	USB Mouse

I: Operating system
In order to run some of the programs to be used you will need to install Raspbian Stretch onto your Micro SD Card.  The best way to do this is not from the RPI but on a different Desktop or Laptop with a SD card port. The link for the OS system is below.

https://www.raspberrypi.org/downloads/raspbian/

Once successfully installed you may insert the Micro SD into the RPI.

II: UCTRONICS LCD Touchscreen Installation
Next on the RPI you must go to the terminal and type the command, 

sudo raspi-config 

Once at the Advanced Operations Page you must select Expand Filesystem.  Now reboot the RPI, this can be done in the terminal with the command 

sudo reboot 

Once rebooted in the terminal use the command 

sudo apt-get update  

When done install the driver package from the terminal with the line: 

sudo git clone https://github.com/UCTRONICS/UCTRONICS_LCD35_RPI.git  

Next compile the directory: 

cd UCTRONICS_LCD35_RPI  

then run the commands (separately):
[1st]:  sudo chmod 777 UCTRONICS_LCD_backup  
[2nd]:  sudo chmod 777 UCTRONICS_LCD35_install  
[3rd]: sudo chmod 777 UCTRONICS_LCD_restore  
[4th]: sudo chmod 777 UCTRONICS_LCD_hdmi 

Afterwards we will finally install the driver for the UCTRONICS screen. Type: 
sudo ./UCTRONICS_LCD_backup  
then: 
sudo ./UCTRONICS_LCD35_install  .
There is a qwerty touchscreen keyboard and screen calibrator feature that may be installed and is recommended, more on this can be found on the UCTRONICS Github page.  There are two important commands needed to switch between the touchscreen display and a HDMI display to use the touchscreen it is:
sudo ./UCTRONICS_LCD_restore
To use a monitor or HDMI source:
sudo ./UCTRONICS_LCD_hdmi 

III:Pure Data
To install Pure Data to the Raspberry pi we should set it a specific directory.  You should follow this command as below

sudo nano /etc/apt/sources.list

Next you will want to type this into the list and or shell that you have opened and created:
deb-src http://archive.raspbian.org/raspbian wheezy main contrib non-free rpi
After you finish typing this in you will need to use ctrl o to save what you have added and then use ctrl b to close the list.  Next, you will once again want to update your RPI with the command

sudo apt-get update

When the update is finished run:

wget https://puredata.info/downloads/pd-extended-0-43-3-on-raspberry-pi-raspbian-wheezy-armhf/releases/1.0/Pd-0.43.3-extended-20121004.deb

When the packages are installed depackage them with:

sudo dpkg –I Pd-0.43.3-extended-2012004.deb
Install them with:

sudo apt-get –f install

This should fully install CsoundQt to your RPI, but to make the program easier to find or open you may run the command

sudo chmod 4755 /usr/bin/pd-extended

I personally have my Pd application available on my desktop but that is my personal preference please move the location of Pd to whatever is easiest for you.  
A few interesting quirks or bugs I have found for Pd that are very important are that of audio connectivity and midi connectivity.  In order to use a midi device with Pd you must have it open before you run the application.  A good way to check if your midi and audio are successfully connected to Pd is in the program go to Media > Test Audio and Midi.  Once on that page Make sure you DSP is running, then go to Test Tones make sure tone is selected dB 80 or 60 is selected.  You should hear a frequency that you can change by dragging the pitch number up and down.  If you hear sound that means that your audio is working otherwise audio should be set to bcm2825.  Midi should be set to Dev Midi, once applied you can check that it works by observing if the stripnote and ctlin objects are generating any sort of messages.  If all these things are working then you have successfully installed Pd!
 
IV:CsoundQT
To install CsoundQT you should first make sure your RPI is up to date by running the command,

sudo apt-get update

once updated run the command

sudo apt-get install csound

Next you will download the CsoundQt program with the command as follows,

sudo apt-get CsoundQt

After this is successfully installed you will want to reboot your RPI (in the terminal sudo reboot).  Next you may want to check your RPI’s soundcard or playback properties. One way to do this is to type

alsamixer

This should bring to the ALSA page on your RPI.  The alsamixer should be running on the bcm2835 soundcard.  Another way to check if your sound is working appropriately is to run the command 

speaker-test

You should get white noise or pink noise panning between the left and right speakers.  One more way to check your sound and to visually see what it is connected to is to run the command a play –l to see the listed specifics of where you sound is coming from.
CsoundQt may be run as by opening the application or for faster use you may run CsoundQt through the terminal.  The command of running csound through the terminal will look something like this
csound -odac -+rtaudio=alsa -B2048 -b2048 /path/to/file.csd
In your own csound instruments or procedures that you design you will want to use a specific set of commands in the csd file under the <CsOptions>.  This will look as such

<CsOptions>
-odac:hw:0 -+rtaudio=alsa -B 2048 -b 2048 -+rtmidi=alsa –Ma
</CsOptions>

V: Sonic Pi
First and foremost Sonic Pi should be already be installed on your RPI.  If it is not the installation process for it is probably the most simple compared to all the others we have touched.  A little disclaimer though I found that Sonic Pi does have a pretty substantial audio issues as well as latency with midi devices on the RPI.  To counter this you may want to get a soundcard and probably a midi adapter (such as the M-Audio 2x2 MidiSport).  To install Sonic Pi through the terminal you will need to run the command

sudo apt-get install sonic-pi

Afterwards you will only be asked to enable a real-time mode with Jack, (I suggest saying yes!). Once this is all done to be safe you might want to reboot the RPI (sudo reboot).  
When you are all done with these processes you should find the application in your programing tab on your desktop.  Open it up so that you may begin to check if everything is set up accurately for midi use and audio playback.  Go to preferences then audio in the Sonic Pi application. Make sure the master volume is pulled up to at least fifty percent.  For synth and Fx safe mode should be enabled.  Next you will want to connect your midi device through USB to your RPI.  Go to IO in the preferences menu.  In midi ports the name of your midi device should be displayed or recognized.  Once this is done got to buffer 0.  Buffers are sort of your different windows or projects that you may use in sonic Pi.  To quick test your send you may use the command play then pick any number 0-127.  This is because play will send the command to Sonic Pi to play a specific MIDI note value.  I recommend using the value of 60 because it is the middle c on piano.  This is also an extremely easy range for people to hear.  The command should look something like this.

Play 60

Once typed use the Run command at the top left corner of the program.  You should hear the midi note 60 played for a short amount of time.  Next we can do a quick check to see if your midi device is connected to Sonic Pi properly. 
Press stop on buffer 0 to completely kill the process and then open up buffer 1. In this new buffer you will type the following (I will be using my Samson Carbon 49 as an example for a midi instrument):

live_loop :midi_piano do
	not,  velocity = sync “/midi/samson_carbon49_midi_1/1/1/note_on”
	synth :piano, note: note, amp:  velocity / 127.0
end  

Press the run button, start playing and viola you should be hearing the midi notes that coincide with you deice as you press them.  Velocity and duration are also taken into account with these commands I have displayed.
For those who wish to dive deeper and learn about the Sonic Pi live coding abilities and language you will be happy to learn that there is a complete help section with tutorials, examples, synths, Fx’s, samples, and languages.

VI: Amsynth
Amsynth is a Linux based open source synthesizer.  To get midi controls on amsynth though you will need Qjackctl.  Qjackctl will help bridge the midi connectivity of your device and amsynth.  Another disclaimer for this specific program is that of which is stated for Sonic Pi, in regards to midi latency issues and sound quality unless you go out and purchase the correct tools for enhancement of these process.
. Type the following command into your terminal:

sudo apt-get install amsynth

Amsynth should be installed after this simple process.  To use Qjackctl you will want to run it from the terminal.  Use the command as follows:

qjackctl

Next you will want to open up Amsynth.  In Qjackctl you will need to make the connection from left to right with your midi device to amsynth.  Once connected you should be hearing the corresponding midi notes that you play on your device with that coming out of Amsynth.

If you want to not use Qjcackctl and only run through ALSA this part is for you! Connect your midi device to your RPI.  Now you need to check which client your midi device is running on.  Type the command in your terminal as follows:

aconnect -o

this will show you what client your Midi Through, Midi device, and Amsynth are running on.  Next you will want to connect you Midi device to amsynth.  For mine it looks like the following, but please add your appropriate numbers for your clients to the command below:

aconnect 20:0 128:0

Now you may check if the connections were made with the command 

aconnect –l

You should see that your midi client says Connecting To your Amynths’s specific client number and the same vice versa in Amsynth’s client.  Open up Amsynth and try out your midi keyboard.  You should hear notes come out, you may also press the auditon button in Amsynth to get a sample of what sound the specific synth should sound like.
After all theses steps you should have amysnth dully functional on your RPI.

VI: Results
Now you should have a fully functioning pocket synthesizer!  The creation of synths and deeper exploration are up too you!  I hoped this helped some people who may have ran into issued trying to get some of these programs to work functionally on their Raspberry Pi’s.  Lastly, if there are any updates or specific issues that arer run into when following this guide please let me know so that I may make the appropriate edits to my documentation!
References

Csound Journal, www.csounds.com/journal/issue18/beagle_pi.html.


Sand, Software and Sound, 15 Feb. 2016, sandsoftwaresound.net/rpi-soft-synth-get-started/.


UCTRONICS. UCTRONICS 3.5 Inches 480 x 320 TFT LCD Touch Screen Display. UCTRONICS, UCTRONICS 3.5 Inches 480 x 320 TFT LCD Touch Screen Display.
http://www.uctronics.com/download/Amazon/U4703.pdf
Csound Journal, www.csounds.com/journal/issue18/beagle_pi.html.






