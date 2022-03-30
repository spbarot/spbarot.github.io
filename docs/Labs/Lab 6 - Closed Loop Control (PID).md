---
title: Lab 6 – Closed-Loop Control (PID)
---
# Author: Swapnil Barot (NetID: spb228)
---

[Return to Main Page](https://spbarot.github.io/)

## I. Objective

The primary objective of this lab is to implement a Closed-Loop Control (PID) system to control the robot positioning with respect to obstacles. Task A – “Don’t Hit the Wall” is selected, which revolves around driving as fast as possible towards a wall, then stopping at exactly 300mm away with the help of ToF sensor readings and the PID controller.

---

## II. Materials/Software

1. 1x SparkFun RedBoard Artemis Nano
2. 1x USB A to C Cable
3. 1x R/C Stunt Car
4. 2x Dual Motor Driver
5. 2x Li-ion 3.7V 650mAh (or more) Battery
6. 2x 4m ToF Sensor
7. 9DOF IMU Sensor
9. 1x Qwiic Connector
10. Measuring Tape

---

## III. Procedure/Design/Results
#### Prelab 

The following tasks shall be accomplished as a part of this lab: 
* Implement a PID/PD controller 
* Setup the BLE framework to send data
* Setup the Jupyter () framework to receive data 
* Integrate PID program with BLE program
* Process, condition, and plot the data in Jupyter

To send and receive data, the BLE and Jupyter framework are first setup. The EString class is utilized to capture and transmit the ToF sensor readings, time stamps, and motor inputs onto Jupyter. Below is an example of the logic. 

```
dist_array[counter] = distance;
time_array[counter] = time_current;
pwm_array[counter] = internalSpeed;
for (int i = 0; i < counter; i++){
      tx_estring_value.clear();
      tx_estring_value.append(dist_array[i]);
      tx_estring_value.append("|");
      tx_estring_value.append(time_array[i]);
      tx_estring_value.append("|");
      tx_estring_value.append(pwm_array[i]);
      tx_characteristic_string.writeValue(tx_estring_value.c_str());
   }
```

For the above code to work, several issues may need to be debugged. For example, buffer overflow is an issue that came up which was solved by limiting the size of the arrays or stopping the program after a certain number of seconds. Another issue that came up was a “hard-fault” error which was caused by storing a float value into a static long value. This was resolved by matching the formats of both values (float values). 

The Jupyter file was setup to receive the data under a notification handler. A CMD command (START_REC) is used to trigger the PID and data recording on the Artemis. Another CMD command (SEND_DATA) is used to stop the PID and data recording and start transmitting the information to Jupyter. The following code is used to represent the described logic. 

```
def notification_handler(uuid, value):
    global distance
    distance=ble.bytearray_to_string(value) 
    print(distance)
ble.start_notify(ble.uuid['RX_STRING'], notification_handler)
ble.send_command(CMD.START_REC, "")
ble.send_command(CMD.SEND_DATA, "")

```
---

#### PID/PD Controller 

A PID controller is a very well-known closed loop control system used for numerous applications. The control scheme operates by providing the required output to a meet a specified setpoint. Sensors are often used to determine the error in the controller so that the feedback can be adjusted accordingly to meet the setpoint. For this lab, a PD controller is utilized which outputs the PWM duty cycle that is used to drive the motors.  Upon trial and error, it was noticed that the controller is the most effective when the KP value is 0.15, and the KD value is 3.0. The KD value is extremely high as it was not deemed effective for lower values. The integrator was not used as it delivered an overshoot. The following code is used to compute the PID output. 
```
float PID_pass(float time_current, float time_previous, float error_previous, float current_error, float I_sum) 
{
return (P*current_error) + (I*I_sum) + ((D*(current_error - error_previous))/(time_current -   time_previous));
}
```



#### Task A – Don’t Hit the Wall! 

Task A involves driving as fast as possible towards a wall, then stopping at exactly 300mm away with the help of ToF sensor readings and the PID controller. Several considerations are explored to come up with a control scheme. 

Speed = 0.8 m/s

---

## IV. Conclusion

The objective of this lab, to demonstrate an open loop control scheme was successfully satisfied. The knowledge gained from this lab in regards to programming the Artemis and interfacing with the dual motor drivers will be very useful in future labs. One issue encountered during the lab was the short battery life of the 850mAh battery (~ 9 minutes). In the future, it will be helpful to keep more batteries in the kit. Overall, the lab was quite smooth and the lab guideline seemed to very helpful. 

---

## V. References

1. [ECE 5960 – Lab 6 Guideline](https://cei-lab.github.io/ECE4960-2022/Lab6.html)

---

[Return to Main Page](https://spbarot.github.io/)



