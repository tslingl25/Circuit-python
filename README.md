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
This assignment includes makeing the light on the aruidno change colors. This is done useing RGB values.


```

import time
import board
import neopixel

pixel_pin = board.NEOPIXEL
num_pixels = 10

ORDER = neopixel.GRB

pixels = neopixel.NeoPixel(
    pixel_pin, num_pixels, brightness=0.2, auto_write=False, pixel_order=ORDER
)


def wheel(pos):
  
    if pos < 0 or pos > 255:
        r = g = b = 0
    elif pos < 85:
        r = int(pos * 3)
        g = int(255 - pos * 3)
        b = 0
    elif pos < 170:
        pos -= 85
        r = int(255 - pos * 3)
        g = 0
        b = int(pos * 3)
    else:
        pos -= 170
        r = 0
        g = int(pos * 3)
        b = int(255 - pos * 3)
    return (r, g, b) if ORDER in (neopixel.RGB, neopixel.GRB) else (r, g, b, 0)


def rainbow_cycle(wait):
    for j in range(255):
        for i in range(num_pixels):
            pixel_index = (i * 256 // num_pixels) + j
            pixels[i] = wheel(pixel_index & 255)
        pixels.show()
        time.sleep(wait)


while True:
    pixels.fill((255, 0, 0))
  
    pixels.show()
    time.sleep(1)
    pixels.fill((0, 255, 0))
    pixels.show()
    time.sleep(1)

    pixels.fill((0, 0, 255))
    pixels.show()
    time.sleep(1)

    rainbow_cycle(0.001) 

```


### Evidence
![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/71349802/135673024-dfb9ae0d-bc9a-481a-8b49-1a96f6482051.gif)


### Wiring
none for this

### Reflection
Codeing is hard for me, It took me a while but I eventually got it. I learnd the importance of getting partners to help.




## CircuitPython_Servo

### Description & Code

This assignment involves makeing a servo turn when you put your hand near it. This type of thing is used in all traffic intersections.
CODE
```
import time
import board
import pwmio
from adafruit_motor import servo

# create a PWMOut object on Pin A2.
pwm = pwmio.PWMOut(board.A2, duty_cycle=2 ** 15, frequency=50)

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

<img src="https://github.com/tslingl25/Circuit-python/blob/main/Images/CapServo.gif?raw=true" alt="ServoDemo" width="500">

### Wiring
Can be seen in the gif above.
### Reflection
It worked, it did this by senseing our bodys interupt the magnetic field put off by the wire. I found that the more wire you have near you hand, the easier it is for the thing to sense your body near it.



## Distance sensor

### Description & Code 
This assignment is to make a light change colors in accordance to rgb values detected by a distance sensor.

```

import time
import board
import adafruit_hcsr04
import neopixel
import simpleio

sonar = adafruit_hcsr04.HCSR04(trigger_pin=board.D5, echo_pin=board.D6)
dot = neopixel.NeoPixel(board.NEOPIXEL, 10, brightness=0.5)

r = 0
g = 0
b = 0


while True:

    try:
        distance = sonar.distance
        print((distance,))

        if distance < 5:
            r = 255
            g = 0
            b = 0
        elif distance > 5 and distance < 20:
            r = simpleio.map_range(distance, 5, 20, 255, 0)
            b = simpleio.map_range(distance, 5, 20, 0, 255)
            g = 0
            r = int(r)
            g = int(g)
            b = int(b)
        elif distance > 20 and distance < 35:
            r = 0
            b = simpleio.map_range(distance, 20, 35, 255, 0)
            g = simpleio.map_range(distance, 20, 35, 0, 255)
            r = int(r)
            g = int(g)
            b = int(b)
        elif distance > 35:
            r = 0
            b = 0
            g = 255
            r = int(r)
            g = int(g)
            b = int(b)
        print(r, g, b)
        time.sleep(0.5)

    except RuntimeError:
        print("uh oh!")
        r = 0
        g = 0
        b = 255
        time.sleep(0.1)

    print(r, g, b)
    dot.fill((r, g, b))
    time.sleep(.05)

```

### Evidence
![ezgif com-gif-maker (2)](https://user-images.githubusercontent.com/71349802/135676611-5a1c0b6f-7671-4068-95bd-8632cc8343c9.gif)

### Wiring
Wiring can be seen in the gif above
### Reflection
This assignment was pretty hard, in particullar the rgb values. It is important to look at the past assignments like hello circuit python to help with this assignment.
