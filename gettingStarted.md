Getting Started
===============

### Connect WeIO to Power
![WeIO Power](images/power.jpg)
There are two ways to bring electricity to the board : using wires from DC transformer (5V only) or using micro USB cable. If you want to start fast use micro USB cable and plug the board into your computer. When using wires make attention to plug them with correct polarity. Powering up the board will result turning on LED power diode. Wait a few moments (about 1 minute) until LED AP stays steady. That means that Linux finished boot process, server is started and you board created AP - Access Point WiFi network that will permit you to enter to IDE Web Interface for the first time.

Choose from available networks on your computer "WeIO MACADDRESSNUMBER" and type inside your web browser http://weio.local:8080. This will prompt you the signup screen. If this address don't work it's possible that you don't have support for Bonjour on your machine. However you can always type http://10.0.0.1:8080 and get in WeIO.
After initial sign up into WeIO, everything is ready. WeIO application is always on port 8080 and user application will be on the stantard http port 80.

### Connect to WeIO and First Time Setup
After WeIO has been plugged to electricity and AP LED diode is lit up it's possible to connect to it's own WiFi.
Choose from available networks on your computer "WeIO rescue" and type inside your web browser http://weio.local:8080 address.
This will prompt you the signup screen. If this address don't work it's possible that you don't have support for Bonjour on your machine.
However you can always type http://10.0.0.1:8080 and get in WeIO. If this problem persists, just click on the button "Soft Reset" on the board.

Please fill in all fields. After setting up root password two additional services will be created : SSH access and SAMBA access. (note that ssh will be accessible by ssh root@weio.local but SAMBA will use weio username and the same password as root, these settings can be changed later) As the default DNS name is weio.local by choosing a new one WeIO will be accessible by somename.local. Finally choosing correct country will affect WiFi regional settings and channel settings for radio.

WeIO IDE environnement is now present on the screen. The next thing to do is to connect WeIO to your local WiFi network or so called STA network.

Feel free to explore WeIO WiFi configurator and try to create your own networks or to connect to existants. Once WeIO connected to Internet it will update it's local time and date, updates will be accessibles and other objects or services on the network can be directly accessed.

### "Hello World" with WeIO
This example shows how to blink white light (by turning on RED, GREEN and BLUE LEDs at the same time).
This is usual hello world program for electronics as they don't have screen to write "hello world".
