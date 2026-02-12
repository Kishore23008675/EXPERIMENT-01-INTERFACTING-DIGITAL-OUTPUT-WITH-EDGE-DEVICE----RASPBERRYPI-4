# EXPERIMENT-01-INTERFACTING-DIGITAL-OUTPUT-WITH-EDGE-DEVICE---(RASPBERRYPI-PI4)
### NAME : KISHORE A
### DEPARTMENT : CSE(IOT)
### REG NO : 212223110022
### DATE OF EXPERIMENT : 04.06.2026

### AIM
To interface a digital output device (LED) with the Raspberry Pi 4 and control it using Python.

## APPARATUS REQUIRED
Raspberry Pi 4
LED (Light Emitting Diode)
330Ω Resistor
IR Sensor
Breadboard
Jumper Wires
USB Cable
 ## THEORY

![Raspberry Pi Pin](https://github.com/user-attachments/assets/19e5a1e7-cb46-4909-ba59-e4f4560cae03)





 
 
 
 ### FIGURE-01 RASPI PI 4 PINOUT DIAGRAM 


The Raspberry Pi 4 Model B is built around a Broadcom BCM2711 system-on-chip that integrates a quad-core ARM Cortex-A72 (64-bit) CPU, VideoCore VI GPU, memory controller, and peripheral interfaces, forming a compact yet complete computer architecture where the SoC connects internally to RAM, USB 3.0 controller, Gigabit Ethernet, HDMI display, and wireless modules. Its 40-pin GPIO header provides a flexible pin configuration consisting of power pins (5 V and 3.3 V), multiple ground pins, and general-purpose input/output pins that operate at 3.3 V logic and can be programmed for digital I/O or alternate functions. Key alternate functions include I²C (SDA, SCL) for sensor communication, SPI (MOSI, MISO, SCLK, CS) for high-speed peripheral interfacing, UART (TX, RX) for serial communication, and PWM for control applications.  For communication, I2C (SDA, SCL), SPI (MOSI, MISO, SCK), and UART (TX, RX) interfaces are mapped across different GPIO pins, allowing seamless connectivity with sensors and peripherals. All GPIO pins support PWM (Pulse Width Modulation), making it useful for motor control, LED brightness adjustment, and sound applications. The BOOTSEL button enables USB mass storage mode for firmware flashing, while the DEBUG pins (SWD interface) provide debugging capabilities. With its low power consumption, flexible GPIO options, and rich interface support, the Raspberry Pi Pico is widely used for IoT, embedded systems, robotics, and automation projects.This architecture and pin multiplexing allow the Raspberry Pi 4 to act as both a general-purpose computing platform and an embedded controller, supporting rapid prototyping, hardware interfacing, and IoT applications.


## Working Principle:
Experiment 1A
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to GP15 via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).

Experiment 1B
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The IR sensor is connected one of the GPIO pins in Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF based on the IR sensor.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to any one GPIO via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).
Connect the IR sensor Vcc to any +5V.
Connect the IR sensor GND to any GND.
Connect the IR sensor OUT to any one GPIO. 

## Experiment 1A – LED Blinking using Raspberry Pi 4

## PROGRAM(python):
```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "ANIERYVXD60GDZV"
CHANNEL_ID = 3241544
THINGSPEAK_URL = "https://api.thingspeak.com/update"

# Set GPIO numbering mode
GPIO.setmode(GPIO.BCM)

# Define LED pin
LED_PIN = 18

# Set GPIO18 as output
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=ANIERYVXD60GDZV&field1={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)

try:
    while True:
        # LED ON
        GPIO.output(LED_PIN, GPIO.HIGH)
        print("LED ON")
        send_to_thingspeak(1)
        time.sleep(15)

        # LED OFF
        GPIO.output(LED_PIN, GPIO.LOW)
        print("LED OFF")
        send_to_thingspeak(0)
        time.sleep(15)

except KeyboardInterrupt:
    print("Program stopped")

finally:
    GPIO.cleanup()
```
## OUTPUT 1A:
## LED OFF:
![e6e9da76-4a8c-4bfc-8340-de327833a535](https://github.com/user-attachments/assets/b1d4a4ac-b7b4-4f07-a2f8-678cf196a8f1)
![6e925f19-e1f5-494a-ab52-da691e2271f1](https://github.com/user-attachments/assets/80b5c6ec-2422-4652-9adb-86736e34af90)
![0399a636-01d4-4a0b-ba48-2ef53e530185](https://github.com/user-attachments/assets/c65716a0-9e1c-4978-8361-97e0e88250ad)


# LED ON:

![9d71d9a0-54ba-4517-bbba-8dfae13cd7e1](https://github.com/user-attachments/assets/56864bf2-1d09-4c89-b39d-aa472b996e57)
![26145c40-44ee-436a-aff3-8505d775f91f](https://github.com/user-attachments/assets/6ca5d40f-e75d-4a97-b27e-a5b86b0318b2)
![b3f23c8e-db9e-45bd-bf1e-57a47ddc0ae2](https://github.com/user-attachments/assets/83de5155-65fe-47e7-9980-030248d8f860)


## Experiment 1B – LED Control using IR Sensor

## PROGRAM(python):

```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "467B08E2B427M1CA"
CHANNEL_ID = 3258944
THINGSPEAK_URL = "https://api.thingspeak.com/update"



# Pin setup
SENSOR_PIN = 23   # Input from sensor
LED_PIN = 18      # Output to LED

# GPIO mode
GPIO.setmode(GPIO.BCM)

# Setup pins
GPIO.setup(SENSOR_PIN, GPIO.IN)
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=467B08E2B427M1CA&field2={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)


print("Sensor + LED system running...")

try:
    while True:
        sensor_value = GPIO.input(SENSOR_PIN)

        if sensor_value == 0:   # Many IR sensors give LOW when object detected
            print("Object Detected! LED ON")
            GPIO.output(LED_PIN, GPIO.HIGH)
            send_to_thingspeak(1)

            time.sleep(15)
        else:
            print("No Object. LED OFF")
            GPIO.output(LED_PIN, GPIO.LOW)
            send_to_thingspeak(0)

            time.sleep(15)

        time.sleep(0.1)

except KeyboardInterrupt:
    print("Stopped by user")

finally:
    GPIO.cleanup()
```
## OUTPUT 1B:
# LED OFF(NO OBJECT DETECTED):
![8c6147fc-47b4-4c26-8752-371aca79bfca](https://github.com/user-attachments/assets/eb523dbe-f534-4021-ae2d-b9a6558dce0e)

![41d12d4c-ff14-4831-be29-d4f112e51614](https://github.com/user-attachments/assets/1567c5f9-0983-44d7-986e-59ffc274daf9)

![d0f99782-d69f-45c7-aabd-93c5c949bf87](https://github.com/user-attachments/assets/ac9b1750-e133-4a99-a38d-9b3cfddc1ee6)


 # LED ON(OBJECT DETECTED):
![462122c5-a0b6-48b4-b9a5-eb0e933a0d3e](https://github.com/user-attachments/assets/d0828f6f-5f74-4533-867d-0adb66370f4b)

![c3ae1e89-0725-4248-b3b2-44d34561cf59](https://github.com/user-attachments/assets/e5221e87-75b8-4829-b1b6-aff2e4914c19)

![fa03a303-0028-4303-9647-aecf72704c24](https://github.com/user-attachments/assets/504687e1-f397-4f27-aba2-38c393b0ab09)


## RESULTS
The LED connected to the Raspberry Pi 4 successfully turns ON and OFF at  user defined time  confirming the proper interfacing of a digital output.
