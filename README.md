

### Description & Code
## Servo_button_Control
# Henry o
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
For a first assignment of the year in a new coding langauge this is one good way to start of the year. With this being a new coding language it couldnt be to hard or else we couldnt do it and they struck a perfect balance beatween using what we already knew from arduino and helping us add on the Python. And while i still struggled with some parts of it i was able to work my way through with the help of my peers. 




### Description & Code
 * **Distance_sensor
 * **Henry 
 * **Color on neo pixel will change based on distance
 * **Code courtesy of julian 
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
## Motor_Control
# Henry
# Make a Motor rotate 



```python
import board
import analogio
from digitalio import DigitalInOut
import time 
import pwmio 
turner = analogio.AnalogIn(board.A1)
motor = pwmio.PWMOut(board.D9)

while True:
    print(turner.value) #show value 
    time.sleep(0.25)
    motor.duty_cycle = turner.value 

```


### Evidence
![ezgif com-video-to-gif (2)](https://github.com/wwright71/engr3/assets/143732572/cf7467b3-c4ce-4a99-bf5c-53ded6fd28f8)
*Credits too Will Whright*


### Wiring.
![image](https://github.com/hotting45/engr3/assets/143732418/753e1b4c-6a32-4865-8684-713a6a2110bd)

### Reflection
HMMMMMMMMMMM i dont like this at all it was too difficult and was probably made harder than my own stupidity. It broke for absolutely no reason at all and i had to re-do it so many damn times. there was no joy in this project and i wanna cry. "Credits to Julian for giving me code that worked and lending me his diagram"

### Description & Code
 * **photointerrupter
 * **henry 
 * **Make a photinterrupter read when intterupted 

```python
import time 
import digitalio
import board
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface

photointerrupter = digitalio.DigitalInOut(board.D2)
photointerrupter.direction = digitalio.Direction.INPUT
photointerrupter.pull = digitalio.Pull.UP
photointerrupter_state = None

interrupt_counter = 0
lcd = LCD(I2CPCF8574Interface(board.I2C(), 0x3f), num_rows=2, num_cols=16)
lcd.set_cursor_pos(0,0)
lcd.print("Interrupt count:") 
now = time.monotonic()

while True: 
    print(interrupt_counter)
    #print(photointerrupter_state)
    #print(photointerrupter.value)
    if not photointerrupter.value and photointerrupter_state is None:
        photointerrupter_state = "Interrupted"
       
    if (photointerrupter.value == True) and (photointerrupter_state == "Interrupted"):
        photointerrupter_state = None
        print("Interrupted")
        interrupt_counter = interrupt_counter +1
    if (now + 0.003) < time.monotonic(): 
        now = time.monotonic()
        lcd.set_cursor_pos(1,0)
        lcd.print(str(interrupt_counter))


  +-
```

### Evidence
![316555199-6fecfcb0-394c-4c73-b7d2-8c21778c7bfe](https://github.com/hotting45/engr3/assets/143732418/bef05dd8-a9fb-43cc-bca6-e32ebc1a7f6c)
**Thanks julian 
### Wiring
![image](https://github.com/hotting45/engr3/assets/143732418/201b1ba9-58d4-4775-87f1-986f15176342)


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

## Multi_part_cylinder 
# Henry
# Many pieces make up the one


### Assignment Description
In this assignment our goal was to build of the cylinder in the center in order too form a multi-part cylinder
### Evidence
 ![image](https://github.com/hotting45/engr3/assets/143732418/3bd0524f-8284-4b27-b977-37d00626baf0)
![image](https://github.com/hotting45/engr3/assets/143732418/cb09068e-812b-4fdc-8a71-39f1a88ee934)

### Part Link 
https://cvilleschools.onshape.com/documents/56f2a8c813312ce6582f4210/w/a0917267fcd7e078a11680fe/e/1196e5971a9e23eb1dda5d82?renderMode=0&uiState=6532950605d4c23820c2212e
### Reflection
I never wanna do this again because oh my god sometimes things went wrong and you had to redo everything. Like the top bit if you messed up one part you would have to do it all over and i despise that. althogh it was quite rewarding too figure out what was going wrong and thanks too the help of everyone around me i was abletoo figure it out. And when things went wrong my peers helped me out with this and when finally getting it done a found some joy in this wonderful little tubr. 

## One_Part
# Henry
# One is the loneliest number


### Assignment Description
In this assignment our goal was to make a one part onshape document in order to prep for the certification exam 
### Evidence
![image](https://github.com/hotting45/engr3/assets/143732418/d2bda2ef-a756-455b-8537-89f69b398bc4)
![image](https://github.com/hotting45/engr3/assets/143732418/568d7ace-8653-4bb6-99ef-26f7220fd89c)
![image](https://github.com/hotting45/engr3/assets/143732418/43b1c744-14ab-4661-ae4b-5ed32c00977d)
![image](https://github.com/hotting45/engr3/assets/143732418/a817426d-57bf-4d03-ac77-303088d8314b)

### Part Link 
https://cvilleschools.onshape.com/documents/5f860a0ef36c999fb466076e/w/8a8fff1d3d39096690b3506e/e/71564d859871e6b875bdd8bc?renderMode=0&uiState=6543b974b9ea18220db2042b
### Reflection
THis was a much easier assignment than the multipart cylinder obviously just because it was just one part. I had fun with this and it was overall just an easy one day asssignment. I struggled a bit at the start with figuring out how everything would come together and the weird angles but with some help i figured it out.


## The_plate
# Henry
# Very Simple plate 


### Assignment Description
In this assignment our goal was to make a plate in order to prepare for the onshape certificate exam 
### Evidence
![image](https://github.com/hotting45/engr3/assets/143732418/5656334c-1aa3-4a8b-b4e8-4aac4fbcec2b)
![image](https://github.com/hotting45/engr3/assets/143732418/e2c0cb8b-90f0-4d91-85db-ec5d0e53a1c4)


### Part Link 
https://cvilleschools.onshape.com/documents/b78a17a8c2373f4d91ded51d/w/ae3b09df0b95fb2665123ed4/e/5ca93e8f7996a2c1288b0a00?renderMode=0&uiState=65450fb56ca5354c20bb29b1
### Reflection
The most free onshape assignemnt ever finishing it in a few minutes and although it was good practice it was still very easy. Its good prep though for the onshape certification exam with the plate having the basic skills necsassary in order to complete it. Would do it again 8/10 

## Multi-part colummn 
# Henry
# A microphone stand?


### Assignment Description
This assignemnt has a goal of making multiple diffrent parts focused around one central piece to make a microphone stand
### Evidence
![image](https://github.com/hotting45/engr3/assets/143732418/4c24a7fc-7509-4a20-8747-ca8eb9f52f77)
![image](https://github.com/hotting45/engr3/assets/143732418/ffb55c4c-f92a-4ad7-a897-74a5e866111f)

### Part Link 
https://cvilleschools.onshape.com/documents/a195619daf67fd79ed9d435b/w/138b6f02ac3ea1f025dc977f/e/24b8b9d8104d61907a247557?renderMode=0&uiState=654c197d913c3b76e251b722
### Reflection
This assignment the goal was to create a microphone stand comprised of four diffrent pieces a circular base, a T shaped top, and a screw all coming together to make the final piece. I struggled a bit with the bottom piece for some reason? i had to restart it because it would flip uspidedown when i tried to fix the extrude costing me a bit of extra time. But other than that it was smooth sailing getting the top bit done very quickly and the screw was a free little bonus piece that took me a few seconds. Overall a very fun assignemnt that was not as hard as the multipart cylinder would rate it a 9/10 

### Description & Code
 * **Rotary encoder
 * **henry 
 * **rotary encodes all 

```python
#import rotaryio
import board 
import neopixel
import digitalio
from lcd.lcd import LCD 
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface 

        
enc =  rotaryio.IncrementalEncoder(board.D4, board.D3, divisor=2)
lcd = LCD(I2CPCF8574Interface(board.I2C(), 0x27), num_rows=2, num_cols=16 )
7
led=neopixel.NeoPixel(board.NEOPIXEL, 1)
led.brightness = 0.3 
led[0] = (255, 0, 0)


button = digitalio.DigitalInOut(board.D2)
button.direction = digitalio.Direction.INPUT
button.pull = digitalio.Pull.UP
button_state = None


while True:
    menu_index = enc.position
    menu_index_lcd = menu_index % 3
    menu = ["stop", "caution", "go"]
    last_index = None
    menu_index = 0 
    menu[menu_index_lcd]
    menu[0] = "stop01
    menu[1] = "caution"        
    menu[2] = "STOP"
    if not button.value and button_state is None:
        button_state = "pressed"
    if button.value and button_state == "pressed":
        print("Button is pressed")
        button_state = None     
    lcd.set_cursor_pos(0,0)
    lcd.print("push for: ")
    lcd.set_cursor_pos(1,0)
    lcd.print("        ")
    lcd.set_cursor_pos(1,0)
    lcd.print(menu[menu_index_lcd])
    if menu_index_lcd == 0:
        led[0] = (255, 0, 0) 
    if menu_index_lcd == 2:
        led[0] = (0, 255, 0) 
    if menu_index_lcd == 1:
        led[0] = (255, 255, 0) 
        
```

### Evidence
https://github.com/hotting45/engr3/assets/143732418/5a7dd842-ac6f-4d7d-b772-e9c81da91056
thank you julian
### Reflection
In this assignment the goal was to make a rotary encoder that would change the light on the board and change what it says on the board based on what the rotary encodewr does. This assignemnt sucked on a diffrent level of annoying because my files would just randomly just stop working every now and then and with no coherant way of being able to fix it. But overall it was relativly easy coding and wiring 


## The_robo_arm
# Henry
# The claw 


### Assignment Description
Make a robot arm that can pick things such as a stack of cards 
### Evidence
![image](https://github.com/hotting45/engr3/assets/143732418/51bb6142-f29f-4dbc-a0dd-1534ed118304)
![image](https://github.com/hotting45/engr3/assets/143732418/4b78c327-fd09-457c-b1a6-6e667f208d5e)
### Part Link 
https://cvilleschools.onshape.com/documents/7cba0d31810a42f9aad7c00f/w/408cd61fe599dccad34ea58a/e/f3b21311ff1bc67460a1a3e3?renderMode=0&uiState=65dcb65d824aca0e1ac6da7d
### Reflection
This was a fun warmup and overall quiet enjoyable overall. And although i had a bit of trouble assmebling it in the assembly tab i just kept ramming my head into the problem until it eventually started workin. This was a fun first onshape assignment in a while 7/10

### Description & Code
 * ** steppa motor
 * **henry 
 * ** motor be steppin 

```python
import rotaryio #takes code from files.
import board #takes code from files.
import neopixel #takes code from files.
import digitalio #takes code from files.
from lcd.lcd import LCD #tells you what file it takes the code from.
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface #tells you what file it takes the code from.

enc = rotaryio.IncrementalEncoder(board.D4, board.D3, divisor = 2) #tells you what pins it use and where to

lcd = LCD(I2CPCF8574Interface(board.I2C(), 0x27), num_rows = 2, num_cols = 16)

led = neopixel.NeoPixel(board.NEOPIXEL, 1)# allows for you to use the led/neopixel
led.brightness = 0.3
led[0] = (255, 0, 0)

button = digitalio.DigitalInOut(board.D2)# the code for the button 
button.direction = digitalio.Direction.INPUT# says that you can use the button as a input
button.pull = digitalio.Pull.UP
button_state = None  

menu_index = 0


while True:
    menu_index = enc.position
    menu = ["stop", "caution", "go"]# the states that the buttion can be in.
    last_index = None
    menu[0] = "stop"
    menu[1] = "caution"
    menu[2] = "go"  
    menu_index_lcd = menu_index % 3
    lcd.set_cursor_pos(0,0)
    lcd.print("Push for: ")
    lcd.set_cursor_pos(1,0)
    lcd.print ("           ")
    lcd.set_cursor_pos(1,0)
    lcd.print(menu[menu_index_lcd])
    print(menu_index_lcd)
    if not button.value and button_state is None:
        button_state = "pressed"
    if button.value and button_state == "pressed":# the state the button is in.
        print("Button is pressed")
        button_state = None
    if menu_index_lcd == 0:
        led[0] = (255, 0, 0)
    if menu_index_lcd == 1:
        led[0] = (255, 255,0)
    if menu_index_lcd == 2:
        led[0] = (0, 255, 0) # makes the led color.
    ```



### Evidence
![ezgif com-cut](https://github.com/Jtoney40/engr3/assets/143732462/831b6171-2d30-473a-97ba-f26f110c41eb)

### Wiring 
![image](https://github.com/Jtoney40/engr3/assets/143732462/5d1d6bba-cdf5-4377-b1e9-29f7a6068e17)

### Reflection 
The assignment was really easy to do and over all no problems. But make sure your wiring is right.

## CircuitPython_Servo

### Description & Code Snippets
The goal of the assignment was to get a servo to move 180 in two diffrent directions with a button. I accomplished the goal by working on the button part first so I could get it to work then I did the servo part then added it all togather.

```python

"""CircuitPython Essentials Servo standard servo example"""
import time
import board
import pwmio
from digitalio import DigitalInOut, Direction, Pull
from adafruit_motor import servo


# create a PWMOut object on Pin A2.
pwm = pwmio.PWMOut(board.A2, duty_cycle=2 ** 15, frequency=50)

btn1 = DigitalInOut(board.D5)
btn1.direction = Direction.INPUT
btn1.pull = Pull.DOWN

prev_state = btn1.value

btn2 = DigitalInOut(board.D2)
btn2.direction = Direction.INPUT
btn2.pull = Pull.DOWN

prev_state = btn2.value
# Create a servo object, my_servo.
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
  +-
        
```
###Credits to will Wright for the code

### Evidence
![image](https://github.com/hotting45/engr3/assets/143732418/b121a223-f9f1-4029-8678-73a7efe27afd)
### Wiring
![image](https://github.com/hotting45/engr3/assets/143732418/e51f430b-af6a-4a13-889a-c8e9d0237071)
### Reflection
Man do i absolutely despise coding sometimes. When everything just stops working randomly and you have no solution and mr Miller just says "You got this" or "I believe in you" i just cant. And its even worse when it just randomly starts working out of nowhere an you cant explain it. Thank you julian for video and wiring diagram 

### Description & Code
 * ** Sensor
 * **henry 
 * ** it senses 

```python

import board
import digitalio
import neopixel 


led = neopixel.NeoPixel(board.NEOPIXEL, 1)
led.brightness = 0.3

ir_sensor = digitalio.DigitalInOut(board.D2)
ir_sensor.direction = digitalio.Direction.INPUT
ir_sensor.pull = digitalio.Pull.UP


led[0] = (255, 0, 0)
led[0](0, 255, 0) 



while True:
        if ir_sensor.value == 0:
                print("Ir sensor is Low")
                led[0] = (255, 0, 0)
            
  

        if ir_sensor.value == 1:
           print("sensor triggerd")
        led[0] = (0, 255, 0)
           

        
```

### Evidence

### Reflection
This was simple in a way that made me think it was complicated to an odd degree. It was in actuality quite simple and overall not that tough. I would rate this a solid 7.5 just because of how hard i made it on myself other than that it was easy. 
