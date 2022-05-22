---
title: Lab 7 – Kalman Filter
---
# Author: Swapnil Barot (NetID: spb228)
---

[Return to Main Page](https://spbarot.github.io/)

## I. Objective

The primary objective Lab 7 is to implement a Kalman Filter with the PID scheme designed in Lab 6. The purpose of employing a Kalman Filter is to accomplish the behavior displayed in Lab 6 at a faster pace. This speed up will assist in completing stunts in the next lab (Lab 8). 

---

## II. Materials/Software

1. 1x SparkFun RedBoard Artemis Nano
2. 1x USB A to C Cable
3. 1x Fully Assembled Robot (Artemis, Batteries, TOF Sensors, IMU)
4. Measuring Tape
5. Obstacle for the Robot (Wooden Dresser)

---

## III. Procedure/Design/Results
#### Prelab 

Lectures 11 and 12 - Implementing a Kalman Filter (see references for links) are reviewed prior to initiating the lab work. 

---

#### 1. Step Response

<img src="../images/Lab7/step_dist.JPG" width="500" alt="image1" class="inline"/>
<img src="../images/Lab7/step_pwm.JPG" width="500" alt="image1" class="inline"/>



---


#### 2. Kalman Filter Setup

Kalman Variables
```
maxvelocity  = 510 mm/s
rise time = 2.0 s
d = 1/maxvelocity
d = 1/510 
m = -d*(rise time)/ln(0.1) = 0.0017
-d/m = -1.15
1/m = 588
delta_t = 1 / sampling_rate
sampling_rate = 79
delta_t = 0.0127
```



Kalman Code
```
A = np.array([[0, 1], [0, -1.15]])
B = np.array([[0], [588]])
C = np.array([[-1, 0]])
Delta_T = 0.0127
A_d = np.identity(2) + Delta_T * A
B_d = Delta_T * B
sig = np.array([[10**2, 0], [0, 10**2]])
sigma_1 = math.sqrt((15**2))
sigma_2 = math.sqrt((15**2))
sigma_3 = (8**2)
sig_u = np.array([[sigma_1**2,0],[0, sigma_2**2]])
sig_z = np.array([[sigma_3**2]])
kf_estimates = []
x = np.array([[-distanceValUp[0]], [0]])
print(A_d)
print(B_d)
```

A and B Matrices

```
A_d = [[1.       0.0127  ]
                 [0.       0.985395]]

B_d = [[0.    ]
            [7.4676]]
```

---


#### 3. Sanity Check - Kalman Filter

50, 50, 5
<img src="../images/Lab7/kf_check_1.JPG" width="500" alt="image1" class="inline"/>

5, 5, 50
<img src="../images/Lab7/kf_check_2.JPG" width="500" alt="image1" class="inline"/>

15, 15, 5
<img src="../images/Lab7/kf_check_3.JPG" width="500" alt="image1" class="inline"/>

---


#### 4. Kalman Filter Implementation on the Robot



<iframe width="560" height="315" src="https://www.youtube.com/embed/IdLj2RFyy4Y" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


---


## IV. Conclusion
The objective of this lab, to implement a Kalman Filter on the robot was successfully satisfied. There were several issues faced during the lab such as hardware faults, software bugs (BLE and hardfaults). Overcoming these challenges was very satisfying and the knowledge gained from this lab in regards to programming the Artemis, interfacing with the motor drivers and sensors, and implementing PID control will be very useful in the future labs. The Lab 7 guideline as well as the staff was also extremely helpful during the lab. 

---

## V. References

1. [ECE 5960 – Lab 7 Guideline](https://cei-lab.github.io/ECE4960-2022/Lab7.html)
2. [ECE 5960 – Lecture 11](https://cei-lab.github.io/ECE4960-2022/lectures/FastRobots-11-LQR-KF.pdf)
3. [ECE 5960 – Lecture 12](https://cei-lab.github.io/ECE4960-2022/lectures/FastRobots-12-KF-Navigation.pdf)

---

[Return to Main Page](https://spbarot.github.io/)

IMAGE: 
<img src="../images/Lab6/dist_time.JPG" width="500" alt="image1" class="inline"/>

VIDEO: 
<iframe width="560" height="315" src="https://www.youtube.com/embed/IdLj2RFyy4Y" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

