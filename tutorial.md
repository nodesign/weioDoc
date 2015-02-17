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

### Creating first HTML UI

### Blink the LED in JavaScript
