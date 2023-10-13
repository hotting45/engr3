

### Description & Code
## Servo_button_Control
# Henry
# Makes a servo smoothly rotate back and forth



```python
import time
import board
import pwmio
from digitalio import DigitalInOut, Direction, Pull
from  adafruit_motor import servo



pwm = pwmio.PWMOut(board.A2, duty_cycle=2 ** 15, frequency=50)

btn1 = DigitalInOut(board.D5)
btn1.direction = Direction.INPUT
btn1.pull = Pull.DOWN

prev_state = btn1.value

btn2 = DigitalInOut(board.D2)
btn2.direction = Direction.INPUT
btn2.pull = Pull.DOWN

prev_state = btn2.value
my_servo = servo.Servo(pwm)

while True:
    if btn1.value:
        print("BTN1  is down")  
        for angle in range(0, 180, 5):  # 0 - 180 degrees, 5 degrees at a time.
            my_servo.angle = angle
      
    if btn2.value:
        print("BTN2  is down") 
        for angle in range(180, 0, -5): # 180 - 0 degrees, 5 degrees at a time.
            my_servo.angle = angle
    
    time.sleep(0.35)

```


### Evidence



### Wiring
![image](https://github.com/hotting45/engr3/assets/143732418/19aa3482-738e-461d-9123-cc379e606869)


### Reflection
I quite enjoyed getting to experiment with this new coding language. the challenge where figuring out how to translate the things I new over into this whole new coding language. 




### Description & Code
 * **Distance_sensor
 * **Henry 
 * **Color on neo pixel will change based on distance 
Get a distance sensor too change the color of the Neopixel based on distance


```python
import time
import board
import adafruit_hcsr04  
from rainbowio import colorwheel
import neopixel 
import simpleio

sonar = adafruit_hcsr04.HCSR04(trigger_pin=board.D5, echo_pin=board.D6)

pixel_pin = board.NEOPIXEL
num_pixels = 1

pixels = neopixel.NeoPixel(pixel_pin, num_pixels, brightness=0.3, auto_write=False)

def color_chase(color, wait):
    for i in range(num_pixels):
        pixels[i] = color
        time.sleep(wait)
        pixels.show()
    time.sleep(0.5)


def rainbow_cycle(wait):
    for j in range(255):
        for i in range(num_pixels):
            rc_index = (i * 256 // num_pixels) + j
            pixels[i] = colorwheel(rc_index & 255)
        pixels.show()
        time.sleep(wait)

RED = (255, 0, 0)
YELLOW = (255, 150, 0)
GREEN = (0, 255, 0)
CYAN = (0, 255, 255)
BLUE = (0, 0, 255)
PURPLE = (180, 0, 255)

while True:
    try:

        print((sonar.distance,))
        if (sonar.distance <= 5.0):
            blue = 0
            green = 0
            red = 255

        if (sonar.distance >= 5.0) and (sonar.distance < 20.0):
            green = 0
            blue = simpleio.map_range(sonar.distance,5.0,20.0,0.0,255.0)
            red = simpleio.map_range(sonar.distance,5.0,20.0,255.0,0.0)

        if (sonar.distance >= 20) and (sonar.distance <35):
            red = 0
            green = simpleio.map_range(sonar.distance,20.0,35.0,0.0,255.0)
            blue = simpleio.map_range(sonar.distance,20.0,35.0,255.0,0.0)
        if (sonar.distance >=35):
            red = 0
            blue = 0
            green = 255
        print("red =",red," green = ",green," blue = ",blue)    
        pixels[0] = (red,green,blue)
        pixels.show()
    
    except RuntimeError:
        print("Retrying!")

    time.sleep(0.1)
```

### Evidence
![ezgif com-video-to-gif (2)](https://github.com/hotting45/engr3/blob/main/ezgif.com-video-to-gif.gif?raw=true)
### Wiring
![image](https://github.com/hotting45/engr3/assets/143732418/e23cc38b-d1a3-4fdd-acf2-603bd078db5a)

### Reflection
I have never had a nice time with the distance sensors always having them bug out on me, but this time thanks to the help of many teachers and a vast amount of information located on the place called "the internet" and my fellow students Julein and Will for helping me in this project. I mainly struggled with getting the Neopixel to show color but even that was quickly resolved thanks to my teamates.
**Credits to Will Wright for the code**

### Description & Code
 * **photointerrupter
 * **henry 
 * **Make a photinterrupter read when intterupted 

```python
from digitalio import DigitalInOut, Direction, Pull
import time
import board

interrupter = DigitalInOut(board.D7)
interrupter.direction = Direction.INPUT
interrupter.pull = Pull.UP

counter = 0

photo = False
state = False

max = 4
start = time.time()
while True:
    photo = interrupter.value
    if photo and not state:
            counter += 1
    state = photo

    remaining = max - time.time()

    if remaining <= 0:
        print("Number of Interrupts that occured:", str(counter))
        max = time.time() + 4
        counter = 0
```

### Evidence

### Wiring
![image](https://github.com/hotting45/engr3/assets/143732418/e3c70285-6c41-4483-961c-cf1edda4eaef)


### Reflection
This was very similar to what i did on arduino for a photointterupter so i mainly just used the code i got on arduino as a refrence. it was a little hard to figure out the printing because im a little stupid but will helped me out with that 
**Credits to Will Wright for the code and the internet**


## Coat_hanger
# Henry
# Make a coat hanger 


### Assignment Description
In this assignment our goal was to create a coat hanger In Onshape using skills that we had learned previously in engineering. And also mix in new skills that we just learned for the pourpose of this assignemnt 
### Evidence
 ![image](https://github.com/hotting45/engr3/assets/143732418/6467cbc1-e384-4bc1-92df-08dd0d9a851c)
 ![image](https://github.com/hotting45/engr3/assets/143732418/c7877415-5712-4328-8786-22ecb001320f)

### Part Link 
(https://cvilleschools.onshape.com/documents/72eeef4074d09830aad222b3/w/c27749ab2ef9f59f94af26ae/e/f0f49ec3326317e195d4c5ff?renderMode=0&uiState=651ed69c613477675da78738)
### Reflection
I quite enjoyed the first onshape assignement of the year. it felt good to get back into it with this assignemnt the perfect mix of challenges and new things. Although i struggled a bit with figuring out thebest places to reflect things and lengths in general. it still helped me to get back into the flow of things after summer break.
&nbsp;

## Swing_Arm
# Henry
# Make a swing arm 


### Assignment Description
In this assignment our goal was to create a Swing arm In Onshape using multiple diffrent sketches and tools while setting a variable sheet 
### Evidence
![image](https://github.com/hotting45/engr3/assets/143732418/2c52d35c-fa97-4c25-aabc-ea2e5a6eba65)
![image](https://github.com/hotting45/engr3/assets/143732418/66a0f1b1-1a7e-49b9-9a96-a74624c3a21e)
### Part Link 
https://cvilleschools.onshape.com/documents/72eeef4074d09830aad222b3/w/c27749ab2ef9f59f94af26ae/e/f0f49ec3326317e195d4c5ff
### Reflection
In this assignment there where a few challenges that i ran in too liek the fact that i just couldnt extrude the sketch for about 15 minutes until randomly it started working because i removed my construction lines and it just started working for some reason. Overall i dislike this project because i struggle with comples thinking due too my lack of a forebrain.
