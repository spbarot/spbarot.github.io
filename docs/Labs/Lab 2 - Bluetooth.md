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
 
<img src="./images/Lab2/Task1_ino.JPG" width="300" height="300" alt="hi" class="inline"/>

<img src="./images/Lab2/Task1_serial.JPG" width="300" height="300" alt="hi" class="inline"/>

<img src="./images/Lab2/Task1_jupyter.JPG" width="300" height="300" alt="hi" class="inline"/>



---

#### Task 2 – Send Three Floats
  

---

#### Task 3 – Notification Handler
  

---

#### Task 4 – BLE Float VS String
  
---

#### ECE 5960 - Task 1 – Effective Data Rate
  
---

#### ECE 5960 - Task 2 – Reliability


---


## IV. Conclusion


---

## V. Code Appendix

#### 


```
test
```

---


## VI. References

---

[Return to Main Page](https://spbarot.github.io/)



