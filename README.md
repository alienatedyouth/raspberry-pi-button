# raspberry-pi-button
Scripts for a power button for Raspberry Pi (Raspbian based installs)

The Raspberry Pi is an extraordinary piece of kit, providing a fairly powerful computing experience for less than £40 that can handle desktop applications, web server builds and, my personal favourite, retrogaming. The only gripe possibly is that there is no dedicated power button which means that, by default, you have to pull out the power supply and put it back in. That being said, with a little bit of tinkering and coding it's pretty easy to add this to your Pi.

These scripts are what I've used for running a power button for my Raspberry Pi 3 B+ running RetroPie but this should also work with any variant OS based off Raspbian.

Required stuff:
2 female to male jumper leads
Momentary push button (must be normally open, a small tac switch should suffice)
Prototype breadboard (if done for temporary)
Soldering equipment (to make a permanent switch)

Step 1 - create a button circuit. A simple circuit is all that's required, attach the leads to the button (either through the breadboard or by soldering the leads to the button). The female connectors of the leads will be attached to the Pi's GPIO.

Step 2 - compile the scripts on your Raspberry Pi. This is usually better done in a SSH terminal session (you will have to allow SSH sessions in the Raspberry Pi's software first). I used Putty on Windows 10, but you can use whatever you're comfortable with. Once logged in, type the following command into the terminal:

sudo nano listen-for-shutdown.py

Copy the contents of the listen-for-shutdown.py file and paste it into the terminal (using Putty on Windows, you right click the terminal window). Ctrl-X to close and press Y to save the file. Back in terminal, type the following commands in:

sudo mv listen-for-shutdown.py /usr/local/bin/
sudo chmod +x /usr/local/bin/listen-for-shutdown.py

This moves the file to the correct location and makes it executable. Next type in:

sudo nano listen-for-shutdown.sh

Same as before, but this time copy and paste the contents of the listen-for-shutdown.sh file, close and save. Type in the following to the terminal:

sudo mv listen-for-shutdown.sh /etc/init.d/
sudo chmod +x /etc/init.d/listen-for-shutdown.sh

Next we need to enable the script to run at startup. The following commands in terminal will do that, as well as start the script now so it's usable:

sudo update-rc.d listen-for-shutdown.sh defaults
sudo /etc/init.d/listen-for-shutdown.sh start

Now that the software side of things is done, we can hook up the button. 

Step 3 - attach the button to the Raspberry Pi. It doesn't matter what lead goes where but one lead should be connected to pin 5 and one to a ground pin. Any of the ground pins will work, but pin 6 is probably easiest as it's right next to pin 5 - the script checks if pin 5 is connected and thus needs to be connected. 

Step 4 - test the button. While the Pi is running, press the button - the system should cleanly shut down and go into a halt state. Press the button again and the Pi should start back up again. 

If that goes to plan then your hardware Raspberry Pi button is complete!
