---
title: Lab 3 – Sensors
---
# Author: Swapnil Barot (NetID: spb228)
---

[Return to Main Page](https://spbarot.github.io/)

## I. Objective

xxx

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

Wires are connected from the Artemis Board to the Two (2) TOF (Time of Flight) sensors and one (1) IMU (Inertial Measurement Unit) Sensor and soldered together to form permanent connections. 
    
---

#### Task 1 – Time of Flight Sensors (Example 5_Wire_I2C)

The first task of the lab is to verify the address of the first TOF sensor. This can be done by compiling the example code found at File -> Examples -> Apollo3 -> Example05_Wire_I2C. It shall be noted that the program only functions correctly when there is one (1) TOF interconnected with the Artemis Board. Because, I already had two (2) TOF sensors and one (1) IMU connected before beginning this example code, the output is not as expected.

As seen the image below, when multiple sensors are connected, the program outputs all the possible addresses (0x1, 0x2….0x7E). If only one TOF sensor was connected, the program would output the address of that sensor.  
 
<img src="../images/Lab3/TOF_1_wirecode.JPG" width="300" height="300" alt="image1" class="inline"/>

---

#### Task 2 – Time of Flight Sensors (Short Distance Mode VS Medium Distance Mode VS Long Distance Mode)
The data sheet for VL53L1X provides information on the three modes (Short Distance, Medium Distance, and Long Distance) of the sensor. The short distance mode has a maximum sampling rate of 50 Hz and is mostly immune to ambient light, but the maximum ranging distance is limited to 1.3m (4.4ft). The medium distance has a maximum sampling rate of 30 Hz and has a maximum ranging distance of 3.0m (9.8ft). The long distance mode has a maximum sampling rate of 30 Hz with a maximum ranging distance of 4m (13.1ft). 
Out of the three modes, it is hypothesized that the long range mode will be the most effective for the robot. While the long range is highly susceptible to the environment, the robot will be functioning in a relatively controlled environment (indoor electronics laboratory). This will minimize most environmental factors influencing the sensors, while providing a (roughly) 4m sensing range which can assist in mapping the surroundings. Also since the robot is extremely fast, it will need to sense walls quicker and from further distances, making the long distance mode more favorable. 
---

#### Task 3 – Time of Flight Sensors (Short Distance Mode VS Medium Distance Mode VS Long Distance Mode)

The TOF sensor is tested using the “ReadDistance” program found at File -> Examples -> SparkFun_VL53L1X_4m_Laser_Distance_Sensor\examples\Example1_ReadDistance. As seen in the image below, the sensor was taped to a laptop and the measured distance from a wall was obtained with both available modes (long distance mode and short distance mode). The graph below displays the measured data. While the short distance mode was more accurate, its maximum range was about 1450mm. The long distance mode was slightly less accurate but could sense up to much higher distances. Testing the sensor against different surfaces/colors also resulted in fairly accurate measurements. 

---    

#### Task 4 – Time of Flight Sensors (Enable Both TOF Sensors)

To enable both TOF sensors, I strategized to interconnect one TOF sensor’s shutdown pin to a GPIO in the Artemis. This will allow for the program to turn off one TOF sensor, change its address (to 0x30), and turn it back it back on. Below is the code that handles this scheme. 
‘’’
digitalWrite(A2, LOW); //Turn TOF sensor off
distanceSensor.setI2CAddress(30); //Set the new I2C address
digitalWrite(A2, HIGH);//Turn TOF sensor on
‘’’

<img src="../images/Lab3/TOF_1_wirecode.JPG" width="300" height="300" alt="image1" class="inline"/>



---
#### Task 5 – Time of Flight Sensors Additional Tasks (Infrared Transmission)

The TOF sensors utilized in this lab use infrared light (lasers) to determine depth/distance information. The sensor emits a signal, which hits an object (wall) and returns to sensor. The time it takes for the signal to get back to the sensor is used to determine the distance of that object. The pros of a TOF sensor (IR based) are the package size (form factor), and low sensitivity to environment. The cons of such a sensor are cost and low sampling frequency. Amplitude based infrared sensor on the other hand measure the distance using the amplitude of the reflected signals, instead of the time. The pros of such a sensor are cost and package size (form factor). The con of these sensors is susceptibility to the environment. 
  
---

#### Task 6 – Time of Flight Sensors Additional Tasks (Timing Budget)

Timing budget of a sensor is described as the programmed time needed by the sensor to perform and report measurement data. The TOF sensor data sheet states that the “setTimingBudget” function sets the timing to perform one range measurement. The “setInterMeasurementPeriod” function sets the delay between two ranging operations. 
<img src="../images/Lab2/Task3_jupyter.JPG" width="300" height="300" alt="hi" class="inline"/>

---

#### Task 7 – Time of Flight Sensors Additional Tasks (Signal and Sigma)

Signal and Sigma are two parameters the driver uses to qualify the ranging measurement. If signal or sigma are outside the typical limits, the ranging is flagged as invalid. As seen in the image below, the Range Status for certain measurements states “Signal Fail” or “Wrapped Target Fail”, indicating a bad sensor read due to exceeding Signal and Sigma limits. This feature can be useful in a fast robot, where the sensor is experiencing high velocities. Inaccurate readings can be effectively flagged and voided. 

<img src="../images/Lab2/Task3_jupyter.JPG" width="300" height="300" alt="hi" class="inline"/>

---


#### Task 8  – Inertial Measurement Unit (Setup the IMU)
 
The program found at File -> Examples -> SparkFun_ICM-20948_ArduinoLibrary-master -> Examples -> Arduino -> Example1_Basics is ran and the output is displayed in the image below. The ADO_VAL value is the value of the last bit of the I2C address of the IMU. It is set to 1 by default. Since the ADR jumped is closed, the ADO_VAL should be set to 0. 

<img src="../images/Lab2/Task3_jupyter.JPG" width="300" height="300" alt="hi" class="inline"/>

---

#### Task  9 – Inertial Measurement Unit (Accelerometer Pitch and Roll)



‘’’
SERIAL_PORT.print("Roll [ ");
Serial.print((atan2(sensor->accY(),sensor->accZ()))*180/M_PI);
SERIAL_PORT.print(" ], Pitch [ ");
Serial.print((atan2(sensor->accX(),sensor->accZ()))*180/M_PI);
‘’’

---

#### Task  10 – Inertial Measurement Unit (Accelerometer Frequency Response)

xxx

---

#### Task  11 – Inertial Measurement Unit (Gyroscope Pitch, Roll, and Yaw)

xxx

---

#### Task  12 – Inertial Measurement Unit (Gyroscope with Filter)

xxx

---

#### Task  13 – Inertial Measurement Unit (Magnetometer Yaw Angle)

xxx

---


## IV. Conclusion

xxx

---

## V. Code Appendix

Please refer to respective tasks in Section III - Procedure/Design/Results for the code. 

---

## VI. References

1. [ECE 5960 – Lab 2 Guideline](https://cei-lab.github.io/ECE4960-2022/Lab2.html)
2. [Jupyter Lab Tutorial](https://cei-lab.github.io/ECE4960-2022/tutorials/jupyter_notebooks.html)

---

[Return to Main Page](https://spbarot.github.io/)



