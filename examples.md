# Examples

## Analog

### analogRead_PY  & analogRead_PY
 
This examples shows you how to read analog input from the physical world using a potentiometer. A potentiometer is a simple mechanical device that provides a varying amount of resistance when its shaft is turned. By passing voltage through a potentiometer and into an analog input on your WeIO, it is possible to measure the amount of resistance produced by a potentiometer as an analog value. In this examples you will monitor the state of your potentiometer using IDE console (python example) and web user interface (html/JS example). 

#### Circuit
![WeIO Fritzing analogRead](images/analogRead_JS&PY.png)

Connect the three wires from the potentiometer to your WeIO board. The first goes to ground from one of the outer pins of the potentiometer. The second goes from 3.3 volts to the other outer pin of the potentiometer. The third goes from analog input 31 to the middle pin of the potentiometer. 

By turning the shaft of the potentiometer, you change the amount of resistance on either side of the wiper which is connected to the center pin of the potentiometer. This changes the voltage at the center pin. When the resistance between the center and the side connected to 3.3 volts is close to zero (and the resistance on the other side is close to 10 kilohms), the voltage at the center pin nears 3.3 volts. When the resistances are reversed, the voltage at the center pin nears 0 volts, or ground. This voltage is the analog voltage that you're reading as an input.

The WeIO has a circuit inside called an analog-to-digital converter that reads this changing voltage and converts it to a number between 0 and 1023. When the shaft is turned all the way in one direction, there are 0 volts going to the pin, and the input value is 0. When the shaft is turned all the way in the opposite direction, there are 3.3 volts going to the pin and the input value is 1023. In between, analogRead() returns a number between 0 and 1023 that is proportional to the amount of voltage being applied to the pin. 

#### Code: analogRead_PY

In the program below, the only thing that you do will in the setup function is to attach myProcess function to main process with the command:

``attach.process(myProcess)``  
 
Next, in myProcess function, you need to set the pin of your analog input and create an infinite loop:

``pin = 31``  
``while True:``

Finally, you need to print the analog value to your monitor every 100 milliseconds. You can do this with the commands **print "your text"**, **annalogRead(pin)** and **delay(ms)**. You can also add some text to your printed string to make it more showy (use commas to concat strings) :

``print "analogRead pin ",pin," = ",analogRead(pin)``  
``delay(100)``


Now, when you open your IDE Monitor you should see a steady stream of numbers ranging from 0-1023, correlating to the position of the pot. As you turn your potentiometer, these numbers will respond almost instantly. 

``` python
from weioLib.weio import *

def setup():
	attach.process(myProcess)

def myProcess():	
	pin = 31
	while True:
		print "analogRead pin ",pin," = ",analogRead(pin)
		delay(100)
```

#### Code: analogRead_JS

In the program below the visible page content, the body, contains only a paragraph with id "phrase". This paragraph will be used to display the potentiometer value :

``<p id="phrase" ></p>``

To update this value you need to use JavaScript and JQuery functions. You can define what to do When page is loaded and websocket opened whit this function :   
``function onWeioReady() { ... }``

In this example you need to set a timer every 100 milliseconds : 

``setInterval(function() { ... }, 100);``

To read the potentiometer value every 100 milliseconds you need to call analogRead() function.

``analogRead(31,pinCallback);``

This functins works like in python but needs an extra parameter, the callback function, that is received message from IDE server to user interface. This callback contains analog value an pin number.

``function pinCallback(data) { ... }``

Using Jquery functions you can update paragraph text.:

``$("#phrase").html("ANALOGREAD VALUE ON THE PIN 31 IS "+ data.data); ``

Now when you open the web user interface you should see a new phrase every 100 millisenconds with potentiometer value.
 
```html
<!--###################################
#                                     #
#   how to have analogRead on WEIO    #
#                                     #
#######################################

# syntax = analogRead(pin,callback) 
# Value returned is in callback function
# the value is between 0 and 1023
# analog pin are between pin 31 and pin 24
--> 

<!DOCTYPE html>
<html lang="en">
<head >
    <script data-main="www/libs/weioLibs" src="www/libs/require.js"></script>
    <link rel="stylesheet" type="text/css" href="myStyle.css">
    <script>
        
        function onWeioReady() {
            // Do something every 100ms
            setInterval(function() {
            //pinCallback will be called when data arrives from server
            analogRead(31,pinCallback);
            }, 100);
        }
        
        function pinCallback(data) {
            // update displayed phrase
            $("#phrase").html("ANALOGREAD VALUE ON THE PIN 31 IS "+ data.data);
        }
    </script>
</head>

<body>
    <p id="phrase" ></p>
</body>

</html>
```
