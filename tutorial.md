Tutorial
===============

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
    # Attaches blinky function to infinite loop
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

### Creating first HTML UI

### Blink the LED in JavaScript
