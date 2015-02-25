Tutorial
===============

### Program Structure
WeIO program structure is following:

- `main.py` -  this is your main Python program (not mandatory if you are making HTML-only program)
- `index.html` - this is your HTML/JS client-side (executed in user browser) program and/or UI (if you have one)


#### `main.py`
This is your main program, writtent entirely in Python. If you want to write HTML-only program (which is not standalone, but can be executed only when user opens browser and access WeIO),
then you do not need this file.

However, if you want to make standalone programs that are independent of user and execute whenever you turn on your WeIO, then this file is mandatory.

Some caracteristics of WeIO Python support are following:
- Entry point of every project is it's `main.py`, so it must exist
- Full standard Python is supported - you can import and use every Python module (using `pip` or `easy_install`)
- WeIO programs are multi-tasking. I.e. you can launch multiple processes at the same time. We are using Python `multiprocessing` library under the hub for this purpose.

##### Import WeIO Python API
This is achieved by adding
```python
from weioLib.weio import *
```
to your program.


##### Tell WeIO Which Processes to Run on Start
This is achived by `setup()` function (that comes from WeIO API).
It is used like this:
```python
def setup():
     # Attaches myProcess function to infinite loop
    attach.process(myProcess)
```

##### Don't forget to write your program
Based on the information you provided with `setup()`, WeIO will expect that process that you have attached exists, i.e. that you have defined your process.

This process is an ordinary Python function that is executed as a separate process *in it's own separate context* (using Python `multiprocessing module`).

Define it as you would usially define Python function

```python
def myProcess():
    # print message to the console
    print("Hello world")
```

Note that this function ends after printing `Hello world` and exits. THis means that your program will be finished after this.
This is needed for some purposes because you can launch unlimited number of processes at the same time with WeIO, so you might want some of them to finish after they did some task.

However, in embedded world it is usual that main process lives forever and keeps looping in the infinite loop.

To achieve this in Python is trivial - just use `while True` construct as described in the `Hello World` example below, in the `Python Programming` section.


#### `index.html`
This is your main graphical interface, but it can be also used for complete control of WeIO device. If you plan to write only Python standalone program for your WeIO,
without graphical UI (for example if human interaction is done only using some buttons), then you do not really need `index.html`.

However, if you plan to add UI to your project, or even use it as the only source of control, then this file is mandatory.

There is two important parts of `index.html`:

- import WeIO JS API by using
```html
<script data-main="www/libs/weioLibs" src="www/libs/require.js"></script>
```

- Use `onWeioReady()` function to add your interactions with electronics from client-side JavaScript, like this

```javascript
// This function is called when DOM is loaded and web socket opened
function onWeioReady() {
    // write your functions here
}
```

All this is explained in greate detail in section `HTML Programming`.

Note that WeIO JS opens WebSocket towards Python server on WeIO board, so that we have bidirectional asynchronous communication - i.e. any event that happens on the board (like some activity on some pin)
can be `pushed` from the WeIO server (i.e. board itself) towards the client (i.e. user browser). So that information in the browser (i.e. your GUI) can be updated immediately.

This is very powerfull concept that avoids long pollings from browser client and permits us to have all information of the HW practically in real-time represented in our UI.


## Python Programming

### Hello World in Console
*Hello World* is more than a tradition. Everyone should, when learning a new programming language or starting to play with a new programmable platform, start with a *Hello world*. WeIO respects the tradition and then, it's normal that this tutorial page starts with it. How things could be otherwise ?!

This example explains how to start with a python script for WeIO, and print *Hello World* in the console. 
Each python scripts must import the weio API. This is done with the following line :

```python
from weioLib.weio import *
```
Every script must also contains a function named *setup*. This function will be automatically called when the script is started. This function acts as the entry point, where *processes* can be started.
```python
def setup():
     # Attaches myProcess function to infinite loop
    attach.process(myProcess)
```
In the example above, a process named *myProcess* will be started. Each time the function *attach.process()* is called, a new process is started. WeIO can start multiple processes in parallel.

Finally, the function *myProcess* will print *Hello world* in the console :
```python
def myProcess():
    # print message to the console
    print("Hello world")
```

The complete script is shown below, and can also be found inside "examples/processes/helloWorld" from the IDE.
```python 
#######################################
#                                     #
#        HELLO WORLD ON WEIO          #
#                                     #
#######################################

# Description: Print "Hello World" message to the console
# WeIO functions: none

from weioLib.weio import *

def setup():
     # Attaches myProcess function to infinite loop
    attach.process(myProcess)
    
def myProcess():
    # print message to the console
    print("Hello world")
```

### Blink the LED in Python
Ok, well, *Hello world* is cool, but not very spectacular. So let's continue with a more concrete example : Blink a LED.

Once again, the script must import the API and contain the *setup* function :
```python
from weioLib.weio import *

def setup() :
    # Executs process called "blinky". You must define this function below.
    attach.process(blinky)
```

As shown above, we will start a process named *blinky*. The goal of this process is to blink a LED. For this example, we will use the inboard RGB LED, connected on pins 18, 19 and 20 (respectively R, G, B).
In order to use the GPIOs, they must be configured. Each GPIOs can be an *input* or an *output*. The configuration is done with the API function *pinMode(pin, mode)*. Knowing that, we can start to write the *blinky* function :
```python
def blinky():
     pinMode(18, OUTPUT)
     pinMode(19, OUTPUT)
     pinMode(20, OUTPUT)
```

The next step is to blink the LED. Writing a value to a GPIO is done with the function *digitalWrite(pin, value)*. So, to blink the LED, we will create an infinite loop wherein the LED will be alternatively switched ON and OFF :
```python
     # Infinite loop
     while True: 
          digitalWrite(18, LOW)
          digitalWrite(19, LOW)
          digitalWrite(20, LOW)
          
          # Wait for 500ms
          delay(500) 
          
          digitalWrite(18, HIGH)
          digitalWrite(19, HIGH)
          digitalWrite(20, HIGH)
          
          # Wait again for 500ms
          delay(500)
```
And that's it ! The LED will start to blink with a frequency of 1Hz.

A slightly different example can be found in "examples/digital/blink_PY" from the IDE.


## HTML Programming

### Creating first HTML UI
WeIO makes it possible to interract with electronics from client-side JavaScrip (i.e. browser).
This make it easy to create rich GUI for various types of electronics products.

Here is a number of steps needed to make a HTML page that can interact with electronics:

- Include WeIO JS libs in your `index.html`

```html
<script data-main="www/libs/weioLibs" src="www/libs/require.js"></script>
```

- There is a special function `onWeioReady()` that is called by WeIO internal API when WeIO is ready to execute your JS functions

```javascript
// This function is called when DOM is loaded and web socket opened
function onWeioReady() {
    // write your functions here
}
```

Note that once including WeIO JS API, you can use many provided functions like `digitalWrite()` for example - please refer to the `JavaSript API` section in the documentation.

How is this possible?

Well, under the hub we implemented what we call "user agents" in Python, that are tottaly transparent to the user. These agents wait in the background for a message from JS.
When you call for example `digitalRead()` or similar function from JavaScript, which message is passed via WebSocket to a pre-written Python user agent on the board, and it executes this function on your behalf
directly on the board (i.e. it interracts with electronics). Then it picks up return value from the HW and pushes it back to JavaScript. This push is possible because WeIO uses WebSockets, a powerful concept that
enables what is called `server push`, i.e. bidirectional asynchronous communication between sever (board) and the client (browser) - as opposed to the traditional concept where client (browser) 
constantly polls server (board) to get updates.

This way *transparently to the user* we gat a functions in client-side JavaScript that can interract with electronics,
and empower users of WeIO to write whole program using only browser.

This is great, because one can write device functionality quicly as he builds user interface in parallel.

### Blink the LED in JavaScript

Here is a full `examples/digital/digitalWrite_JS` that demonstrates how we can make UI for our device and at the ame time embed functionality inside it, i.e.
interact with electronics from our UI. This makes it sufficient only to use HTML to make some useful products.

```html
<!--###################################
#                                     #
#  HOW TO HAVE DIGITAL WRITE ON WEIO  #
#                                     #
#######################################
# This example shows how to write digital values to PIN's 18, 19 & 20 
# (connected on RGB LED on the board)
# syntax = digitalWrite(pin,value)
# Function available in all PIN's. Value is 0 and 1 or LOW and HIGH
--> 

<!DOCTYPE html>
<html lang="en">

<head >
	<!-- Import weioLibs and link your css style -->
    <script data-main="www/libs/weioLibs" src="www/libs/require.js"></script>
    <link rel="stylesheet" type="text/css" href="myStyle.css">

    <script>
		// init light variable
        var light = false;
        
        // This function is called when DOM is loaded and web socket opened
        function onWeioReady() {
            lights();
        }
        
        function lights() {
            if (light) {
                // Change displayed phrase, background and font color.
                $("#phrase").html("TURN OFF THE LIGHT!");
                $("body").css("background","white");
                $("#phrase").css("color","black");
				// write LOW on pins
                digitalWrite(18, LOW);
                digitalWrite(19, LOW);
                digitalWrite(20, LOW);
				// toggle light variable
                light = !light;
            } else {
                // Change displayed phrase, background and font color.
                $("#phrase").html("TURN ON THE LIGHT!");
                $("body").css("background","black");
                $("#phrase").css("color","white");
				// write HIGH on pins
                digitalWrite(18, HIGH);
                digitalWrite(19, HIGH);
                digitalWrite(20, HIGH);
				// toggle light variable
                light = !light;
            }
        }
    
    </script>
</head>

<body>
    <p id="phrase" onclick="lights()"></p>
</body>

</html>
```
