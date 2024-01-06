A trigger box that sends and receives trigger signals from USB. It can also send (TTL) triggers to MagVenture TMS amplifers via a DB9 interface.

## 1. Arduino nano circuit
The circuit below shows the schematics of a trigger box using Arduino Nano. 
![](https://github.com/fahsuanlin/labmanual/blob/master/images/Schematic_trigger_box_arduino_nano_2023-12-18.png) 

## 2. Triggers inputs and outputs
The trigger box can receive or send triggers. It blinks "blue" lights when sends one trigger. It blinks "green" lights when receives one triggers. Each trigger was set with a duration of 10 ms.

A toggle switch can bridge the received triggers to outputs.  

**NOTE:** Tigger manually outputs were set to have debounce time of 1 second. 

## 3. Matlab interface

[This script](https://github.com/fahsuanlin/arduino/blob/main/trigger_monitor.mlapp) with [this function](https://github.com/fahsuanlin/arduino/blob/main/trigger_monitor_readSerialData.m) is a Matlab App sending and receiving triggers. 

**NOTE:** In this App, "output" refers to the outputs from a computer and the "input" refers to inputs toward a computer.

![](https://github.com/fahsuanlin/labmanual/blob/master/images/trigger_box_matlab.png) 

To use this App.
1. Call `trigger_box` in Matlab. (Make sure your path includes the two Matlab files listed above).
2. Connect the trigger box with USB
3. Choose the port. At Mac, it is like `cu.usbserial-xxxxxx`. 
4. Turn on the port by clicking the switch "On". If a green light shows up, the program is ready to work.
5. To send triggers (TTL outputs), press the `X` button. A counter and a timer shows the output triggers. The `R` button resets the counter and trigger.
6. Received triggers (TTL intputs) will be automatically logged in the counter and timer at `TTL input` row. The `R` button resets the counter and trigger.

