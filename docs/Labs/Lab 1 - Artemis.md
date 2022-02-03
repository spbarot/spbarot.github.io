---
title: Lab 1 - Artemis Board
---
## I. Objective

The goal of this lab is to configure the Sparkfun RedBoard Artemis Nano Board (microcontroller) and the Arduino IDE. Upon verifying the system functionality, the experimenters shall compile and upload example code (with modifications) such as Blink It Up, Serial, AnalogRead, and MicrophoneOutput. Lastly, ECE 5960 students shall develop a program to enable the on-board LED when the microphone senses a whistle. Refer to the Sparkfun RedBoard Artemis Nano datasheet and Artemis setup instructions in the appendix section for preliminary information required for the lab. 

---
## II. Materials/Software

* 1x SparkFun RedBoard Artemis Nano
* 1x USB A to C Cable
* Arduino IDE (Software)

---
## III. Procedure/Design/Results

#### System Setup:

First download and install the Arduino IDE. Then, Interconnect the Artemis and the computer via the USB cable. Once the program is installed, hover to Tools and select Boards Manager. Download and install the Sparkfun Apollo 3 Boards package. 

---
  ### III.II. Blink It Up Example
  
  Once the system is setup, select File, Examples, 01. Basics and open Blink. Analyze the software and select Upload on the toolbar. 
The Blink program enables the on-board blue LED. As per the algorithm, the LED is toggled on and off every 1000 milliseconds. Note that the LED on/off frequency can be modified by adjusting the delay. The video showcases a delay of 100 milliseconds, increasing the on/off frequency. The on-board LED is an extremely useful tool when debugging and verifying software functionalities. 

  <iframe width="560" height="315" src="https://www.youtube.com/embed/FCxQfeuSuMI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  
---

  ### III.III. Serial Example
  
  Select File, Examples, Apollo 3, and open Example04_Serial. 
The serial program verifies the functionality of the serial communication, serial monitor and serial port. The program prints out an incrementing print statement and prompts the user to enter data which echoes in the serial monitor. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/QihvJWqAIBk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---
  ### III.IV. AnalogRead Example
  
  Select File, Examples, Apollo 3, and open Example02_AnalogRead.
AnalogRead takes advantage of the on-board ADC (analog to digital converter) and can read analog voltages from 0V to 2V. The ADC channel measures the internal die temperature, VCC voltage, and VSS voltage. Additionally, the program is designed to fade the on-board LED to match the voltage reading on the respective analog pin. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/N4dWy57jVsg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

  ### III.V. MicrophoneOutput Example
  
  Select File, Examples, PDM, and open Example1_MicrophoneOutput.
MicrophoneOutput is designed to enable the PDM microphone to continuously read input and output the loudest frequency detected. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZEnoK6dK2mE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

  ### III.VI. Microphone_LED
  
  Microphone_LED is a program that enables the on-board LED when the microphone senses a whistle. To carryout this design intent, an if/else statement is utilized that turns the LED on if a frequency of > 4000 Hz is detected and turns the LED off if the recorded frequency is <=4000 Hz. Refer to the appendix section for the code utilized in Microphone_LED. 
  
  <iframe width="560" height="315" src="https://www.youtube.com/embed/J9VLsoFVJos" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  
---

## IV. Conclusion

The experimenters successfully configured the Artemis board and the Arduino IDE while running several example programs to verify the system functionality. Knowledge gained in this lab with respect to system setup, microcontroller familiarity, on-board peripherals, and programming will assist the experiments in future labs. Most of the experiment was quite smooth, except for AnalogRead, which contained bugs that did not allow the software to present the computed/processed data. Instead, the program prints raw data in the form of bits. The lab writeup and the given references proved to be helpful throughout the entire lab. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/FCxQfeuSuMI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## V. Code Appendix

'''

test
test2
test3

'''

---


## VI. References

1. [SparkFun RedBoard Artemis Nano]( https://www.sparkfun.com/products/15443)

2. [Setup Instructions]( https://learn.sparkfun.com/tutorials/artemis-development-with-arduino?_ga=2.30055167.1151850962.1594648676-1889762036.1574524297&_gac=1.19903818.1593457111.Cj0KCQjwoub3BRC6ARIsABGhnyahkG7hU2v-0bSiAeprvZ7c9v0XEKYdVHIIi_-J-m5YLdDBMc2P_goaAtA4EALw_wcB)
