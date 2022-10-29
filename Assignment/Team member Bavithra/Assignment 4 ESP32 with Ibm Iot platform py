import time                                 #importing necessary modules for cloud connectivity and initiating GPIO Pins
import sys
import ibmiotf.application                 
import ibmiotf.device
from machine import Pin
import utime

#IBM Cloud Credentials
organization = "jtp3hb"
deviceType = "ESP32"
deviceId = "123456789"
authMethod = "token"
authToken = "1234567890"

#Intiating Pins for Ultrasonic sensors (Trigger and Echo Pins)
trigger = Pin(3, Pin.OUT)
echo = Pin(2, Pin.IN)


#Try and Except Statement for connecting cloud
try:
    deviceOptions = {"org": organization, "type": deviceType, "id": deviceId, "auth-method": authMethod, "auth-token": authToken}
    deviceCli = ibmiotf.device.Client(deviceOptions)
except Exception as e:
    print ("Caught exception connecting device %s" %str(e))
    sys.exit()

#Device CLI Connectivity
deviceCli.connect()


#Sensing Distance and Alerting Cloud
while True:

    trigger.low()
    utime.sleep_us(2)
    trigger.high()
    utime.sleep_us(5)
    trigger.low()
    while echo.value() == 0:
        signaloff = utime.ticks_us()
    while echo.value() == 1:
        signalon = utime.ticks_us()
    timepassed = signalon - signaloff
    distance = (timepassed * 0.0343) / 2

    if (distance <= 100):

        data = {'temperature': distance}

        def myOnPublishCallback():
            print ("Published temperature")

        success = deviceCli.publishEvent("IoTSensor", "json", data, qos=0, on_publish=myOnPublishCallback)
        if not success:
            print("Not connected")
        time.sleep(1)

deviceCli.disconnect()
