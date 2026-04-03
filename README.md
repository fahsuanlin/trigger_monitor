# A trigger box that sends/receives trigger signals from USB/BNC (Arduino micro version)

This document outlines the final specifications and operational logic for the **TTL Trigger Box** (Revision Date: 2026-04-02). This device is designed for synchronization between experimental software and external hardware (e.g., MRI, EEG, or TMS).

## ---

**1\. Hardware Interface Mapping**
![](https://github.com/fahsuanlin/trigger_monitor/blob/master/Schematic_trigger_box_arduino_micro.png) 

Based on the **12:17 AM Schematic**, the following physical ports are mapped to the Arduino Pro Micro logic. Note that **both J1 and J2 act as outputs** in this configuration.

| Component | Schematic Label | Arduino Pin | Role | Description |
| :---- | :---- | :---- | :---- | :---- |
| **Sense Trigger** | J3, Banana, KEY1 | **A1 (19)** | **Input** | Monitors incoming triggers/manual button presses. |
| **Logic Switch** | SW1 | **A2 (20)** | **Control** | Enables/Disables bridging from Input to J2. |
| **Trigger Out 1** | J1 / Blue LED (U3) | **4 (A6)** | **Output** | Fires a pulse and blinks **Blue** whenever an input is sensed. |
| **Trigger Out 2** | J2 / Green LED (U4) | **5** | **Output** | Fires a pulse and blinks **Green** on Serial or Bridged trigger. |

## ---

**2\. Input/Output & Light Logic**

The box uses two separate light channels to distinguish between **Received** and **Delivered** signals.

| Event | Blue LED (U3) | Green LED (U4) | Physical Output | PC HID Action |
| :---- | :---- | :---- | :---- | :---- |
| **Manual Button Press** | **Blinks** | OFF\* | Pulse via **J1** | Sends Key **'5'** |
| **External Trigger (J3)** | **Blinks** | OFF\* | Pulse via **J1** | Sends Key **'5'** |
| **Forwarding (SW1 ON)** | **Blinks** | **Blinks** | Pulse via **J1 AND J2** | Sends Key **'5'** |
| **Serial Cmd (USB)** | OFF | **Blinks** | Pulse via **J2** | None |

*\*Unless SW1 is in the "Bridge" position.*

## ---

**3\. Connector Definitions**

### **Input Ports (Green Net)**

* **J3 (RG58/BNC):** Primary input for external TTL pulses.  
* **Banana 1.2:** Lab-lead input for manual or custom hardware triggers.  
* **KEY1:** On-board 4-pin tactile button for manual test triggering.

### **Output Ports (Blue & Green Nets)**

* **J1 (Blue RG58):** Fires a "confirmation" pulse every time the box receives an input. Useful for daisy-chaining triggers to a second recording device (e.g., an EEG amp).  
* **J2 (Green RG58):** The primary output for PC-initiated triggers or bridged signals.

## ---

**4\. Software Parameters**

* **Baud Rate:** 9600 bps.  
* **Input Threshold:** \> 500 (Analog).  
* **Pulse Duration:** \* J2 (Output): 200ms.  
  * J1 (Input Notification): 10ms (Adjustable via ttl\_input\_duration\_time).  
* **Debounce/Refractory Period:** 200ms to prevent multiple keystrokes from a single noisy pulse.

## ---

**5\. Maintenance & Safety**

* **Contention Warning:** Since **J1** is configured as an **Output**, do not plug an active pulse generator into J1. This port is meant to *send* signals to high-impedance inputs only.  
* **USB Reliability:** Due to the fragility of the Pro Micro USB socket, ensure the cable is strain-relieved or use the provided pigtail modification.  
* **Signal Voltage:** Standard **5V TTL**. Using triggers above 5V will damage the microcontroller.






# A trigger box that sends and receives trigger signals from USB (Arduino nano version)

This trigger box can also send (TTL) triggers to MagVenture TMS amplifers via a DB9 interface.

This is a copy of [the wiki page at labmanual](https://github.com/fahsuanlin/labmanual/wiki/44.-Trigger-box).

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

