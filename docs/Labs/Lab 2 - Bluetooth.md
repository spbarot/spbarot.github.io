---
title: Lab 2 – Bluetooth
---
# Author: Swapnil Barot (NetID: spb228)
---

[Return to Main Page](https://spbarot.github.io/)

## I. Objective

The goal of this lab is to understand the Bluetooth implementation used to interconnect the computer (off-board computing) and the Artemis board (on-board computing), while configuring and establishing Bluetooth communication. Lab tasks involve installing and configuring a virtual environment, python, and Jupyter, configuring Bluetooth communication between the computer and the Artemis board, sending, and receiving string/float values, and creating a notification handler. 

---

## II. Materials/Software

1. 1x SparkFun RedBoard Artemis Nano
2. 1x USB A to C Cable
3. Computer
3. Arduino IDE (Software)
4. JupyterLab (Software)

---
## III. Procedure/Design/Results

#### System Setup

First download/upgrade Python 3 and pip. Then install a virtual environment. Install JupyterLab and follow the JuputerLab tutorial as required. Now, setup the Artemis board by installing ArduinoBLE from the library manager. Once all the software is installed, configure and match the MAC address and UUIDs of the Artemis board. 
    
---

#### Task 1 – Send an ECHO Command

The first task involves sending an “ECHO” command with a string value from the computer to the Artemis. The Artemis then receives the command and sends an augmented string back to the computer. As show in the images below, CMD.ECHO is utilized to send a string (Hello SPB’s Robot) to the robot (Artemis). The Artemis then sends the string back to the computer (SPB’s Robot Says -> Hello SPB’s Robot (Received From Robot)).   
 
<img src="./spbarot.github.io/docs/images/Lab2/Task1_ino.JPG" width="300" height="300" alt="hi" class="inline"/>

<img src="./images/Lab2/Task1_serial.JPG" width="300" height="300" alt="hi" class="inline"/>

<img src="./images/Lab2/Task1_jupyter.JPG" width="300" height="300" alt="hi" class="inline"/>


---

#### Task 2 – Send Three Floats

Task 2 involves sending three float values to the Artemis board using the SEND_THREE_FLOATS command. “ble.send_command” is utilized to transmit three float values to the Artemis. The Artemis extracts the values using “robot_cmd.get_next_value”. The images below display the program and the serial output in detail.  

<img src="./images/Lab2/Task2_ino.JPG" width="300" height="300" alt="hi" class="inline"/>

<img src="./images/Lab2/Task2_serial.JPG" width="300" height="300" alt="hi" class="inline"/>

<img src="./images/Lab2/Task2_jupyter.JPG" width="300" height="300" alt="hi" class="inline"/>
  

---

#### Task 3 – Notification Handler
 
A notification handler is setup to receive float values from the Artemis board. In the callback function, a float value is stored as a global variable such that it is updated every time the characteristic value changes. This eliminates the need to utilize the receive_float() function. 

<img src="./images/Lab2/Task3_jupyter.JPG" width="300" height="300" alt="hi" class="inline"/>

---

#### Task 4 – BLEFloat VS BLEString

Receiving a float value in python using receive_float() / BLEFloatCharacteristic enables python to directly receive a float value as a byte array (as transmitted by Artemis). On the other hand, using receive_string() / BLECStringCharacteristic forces python to convert the characters to floats in a byte array. The first option (float) requires 4 bytes (per float) while the second option (string) requires 1 byte (per character). Thus, the float values are more memory expensive, however, can lead to more precision.  It shall be noted that both options produce similar results. 

---

#### ECE 5960 Additional Tasks - Task 1 – Effective Data Rate
  

<img src="./images/Lab2/Task5_jupyter.JPG" width="300" height="300" alt="hi" class="inline"/>

<img src="./images/Lab2/Task5_output.JPG" width="300" height="300" alt="hi" class="inline"/>

<img src="./images/Lab2/Task5_graph.JPG" width="300" height="300" alt="hi" class="inline"/>


---
#### ECE 5960 Additional Tasks - Task 2 – Reliability

<img src="./images/Lab2/Task5_output.JPG" width="300" height="300" alt="hi" class="inline"/>

---

## IV. Conclusion


---

## V. Code Appendix


```
test
```

---


## VI. References

---

[Return to Main Page](https://spbarot.github.io/)



