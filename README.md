# CircuitPython
This repository will actually serve as a aid to help you get started with your own template.  You should copy the raw form of this readme into your own, and use this template to write your own.  If you want to draw inspiration from other classmates, feel free to check [this directory of all students!](https://github.com/chssigma/Class_Accounts).
## Table of Contents
* [Table of Contents](#TableOfContents)
* [Hello_CircuitPython](#Hello_CircuitPython)
* [CircuitPython_Servo](#CircuitPython_Servo)
* [CircuitPython_LCD](#CircuitPython_LCD)
* [NextAssignmentGoesHere](#NextAssignment)
---

## Hello_CircuitPython

### Description & Code
For this assignment we had to learn vscode and we made a button blink red

Here's how you make code look like code:

```python
Code goes here
import board
import neopixel
import time
dot = neopixel.NeoPixel(board.NEOPIXEL, 1)
dot.brightness = 0.5 

print("Make it red!")

while True:
    dot.fill((255, 0, 0))
    print("Red")
    time.sleep(0.5)

    dot.fill((255, 0, 255))
    print("Purple")
    time.sleep(0.5)
```


### Evidence



https://user-images.githubusercontent.com/112961434/193046589-a9bb4d3b-664a-4453-8a38-d1be18a54cc4.mp4




And here is how you should give image credit to someone, if you use their work:

Image credit goes to matthias



### Wiring
https://www.tinkercad.com/things/l74pWa2Bojx-brave-turing/editel?tenant=circuits

### Reflection
I got some help from Mr H in order to learn python and vscode.




## CircuitPython_Servo

### Description & Code
this assignment we were asked to move a servo 180 degrees
```python
Code goes here
# SPDX-FileCopyrightText: 2018 Kattni Rembor for Adafruit Industries
#
# SPDX-License-Identifier: MIT

"""CircuitPython Essentials Servo standard servo example"""
import time
import board
import pwmio
from adafruit_motor import servo

# create a PWMOut object on Pin A2.
pwm = pwmio.PWMOut(board.D7, duty_cycle=2 ** 15, frequency=50)

# Create a servo object, my_servo.
my_servo = servo.Servo(pwm)

while True:
    for angle in range(0, 180, 5):  # 0 - 180 degrees, 5 degrees at a time.
        my_servo.angle = angle
        time.sleep(0.05)
    for angle in range(180, 0, -5): # 180 - 0 degrees, 5 degrees at a time.
        my_servo.angle = angle
        time.sleep(0.05)

```

### Evidence


https://user-images.githubusercontent.com/112961434/193046467-25b1430d-01c0-4595-9e3e-dfa70ef56894.mp4



### Wiring
![image](https://user-images.githubusercontent.com/112961434/192555694-4ad0eed2-1c11-4fa1-b789-35e882e869b6.png)
Kattni Rembor, Jeff Epler, Carter Nelson, lady ada
### Reflection
it was a lot more challenging to use a servo because I had to rememeber how to wire and use a servo but google was my friend.


## UltraSonic Senor

### Description & Code
we had to make an ultrasonic sensor that faded the color of the board LED
```
#thanks graham, grant, and mason for the code
import time
import board
import adafruit_hcsr04
import neopixel
import simpleio

sonar = adafruit_hcsr04.HCSR04(trigger_pin=board.D3, echo_pin=board.D2)
matthias = neopixel.NeoPixel(board.NEOPIXEL, 1)#connecting the neopixel on the board to the code
matthias.brightness = .3  #setting the brightness of the light, from 0-1 brightness

while True:
    try:
        cm = sonar.distance
        simpleio.map_range(cm, 0, 20, 3, 20)
        print((sonar.distance))
        if cm < 7.5:
            red = simpleio.map_range(cm, 0, 6.5, 255, 0)# mapping the colors to the length
            green = simpleio.map_range(cm, 5, 7.5, 0, 230)
            matthias.fill((red, green, 0))
        if cm > 7.5 and cm < 12.5:
            green = simpleio.map_range(cm, 7.5, 10, 255, 0)
            blue = simpleio.map_range(cm, 9, 12.5, 0, 230)
            matthias.fill((0, green, blue))
        if cm > 12.5 and cm < 17.5:
            blue = simpleio.map_range(cm, 12.5, 15, 255, 0)
            red = simpleio.map_range(cm, 14, 17.5, 0, 240)
            matthias.fill((red, 0, blue))
        time.sleep(0.01)
    except RuntimeError:
        print("Retrying!")
    time.sleep(0.1)

### Evidence
```



https://user-images.githubusercontent.com/112961434/193046166-11c7c9fa-54c6-4ee5-ade7-2176b5cff94f.mp4






And here is how you should give image credit to someone, if you use their work:

Image credit goes to matthias



### Wiring
![193045742-26a5ac02-6881-416c-9d54-af293deceae0](https://user-images.githubusercontent.com/112961434/193048819-4750fa1a-3b1d-4859-a733-ea9df81ee28b.png)
elias https://github.com/egarcia28/CircuitPython
### Reflection
this assignment was difficult because of the fading of the light


## CircuitPython_LCD

### Description & Code

```python
Code goes here
#Grant Gastinger https://github.com/ggastin30/CPython
#lcdAssignment based off 
#Uses an LCD to display the amount of times a button is clicked. Reverses if switch is flipped.

import board
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
import time
from digitalio import DigitalInOut, Direction, Pull

# get and i2c object
i2c = board.I2C()
btn = DigitalInOut(board.D2)
btn.direction = Direction.INPUT
btn.pull = Pull.UP
clickCount = 0

switch = DigitalInOut(board.D7)
switch.direction = Direction.INPUT
switch.pull = Pull.UP

# some LCDs are 0x3f... some are 0x27...
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)

lcd.print("Grant")

while True:
    if not switch.value:
        if not btn.value:
            lcd.clear()
            lcd.set_cursor_pos(0, 0)
            lcd.print("Click Count:")
            lcd.set_cursor_pos(0,13)
            clickCount = clickCount + 1
            lcd.print(str(clickCount))
        else:
            pass
    else:
        if not btn.value:
            lcd.clear()
            lcd.set_cursor_pos(0, 0)
            lcd.print("Click Count:")
            lcd.set_cursor_pos(0,13)
            clickCount = clickCount - 1
            lcd.print(str(clickCount))
        else:
            pass
    time.sleep(0.1) # sleep for debounce
```

### Evidence


https://user-images.githubusercontent.com/112961434/193046801-679b4d02-3eee-4f74-8c34-5ac0fc597b24.mp4


### Wiring
![193033429-e5198fd6-79fd-4952-a702-64e0c3bba90c](https://user-images.githubusercontent.com/112961434/193047265-43d4e611-0c68-453c-8ac3-ebb8ee76e326.png)
###thanks elias https://github.com/egarcia28/CircuitPython
### Reflection
this was by far the hardest assignment but I had done something like this last year and my prior knowledge along with the help of classmates gave me what I needed to understand and complete the assignment.




## NextAssignment

### Description & Code

```python
Code goes here

```

### Evidence

### Wiring

### Reflection
