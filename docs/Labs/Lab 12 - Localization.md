---
title: Lab 12 – Localization on the Real Robot
---
# Author: Swapnil Barot (NetID: spb228)
---

[Return to Main Page](https://spbarot.github.io/)

## I. Objective

The primary objective of this lab is to perform localization on the real robot, using only the update step of the Bayes Filter. Localization is the process of determining where a robot is located with respect to its environment. 

---

## II. Materials/Software

1. Fully Assembled Robot (Artemis, ToF Sensors, IMU, Motor Drivers)
2. Jupyter Lab

---

## III. Procedure/Design/Results

#### Setup

Lab 11 Bayes filter implementation and the provided optimized Bayes filter implementation are utilized to perform localization on the virtual robot. Once the localization on the virtual robot is successful, the real robot undergoes localization.  For this lab, the robot is placed in the following 4 marked positions on the map to attain sensor readings:
<br>
* (-3 ft ,-2 ft ,0 deg)
* (0 ft ,3 ft ,0 deg)
* (5 ft ,-3 ft ,0 deg)
* (5 ft ,3 ft ,0 deg)
<br>

At the abovementioned locations, the Tof sensors shall take 18 sensor readings at 20 degree increments starting from 0 degrees to 340 degrees (0, 20, 40, …, 340). These readings shall be passed into the RealRobot class module in Python where the 18 sensor readings are stored and utilized in the update step to determine the robot’s current location and belief level. 

---

#### Task 1 – Test Localization in Simulation

The provided localization implementation is first tested to confirm capability, before proceeding to the real robot. As seen in the plot below, the ground truth (blue), and the robot belief (green) are very similar indicating that the software demonstrates accurate implementation.   

<img src="../images/Lab12/sim.JPG" width="500" alt="image1" class="inline"/>

---



#### Task 2 – Update Step on Real Robot 




<iframe width="560" height="315" src="https://www.youtube.com/embed/uWDAeCTHezg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<img src="../images/Lab12/0,3.jpg" width="500" alt="image1" class="inline"/>
<img src="../images/Lab12/-3,-2.jpg" width="500" alt="image1" class="inline"/>
<img src="../images/Lab12/5,3.jpg" width="500" alt="image1" class="inline"/>
<img src="../images/Lab12/5,-3.jpg" width="500" alt="image1" class="inline"/>



---

## IV. Conclusion

The objective of this lab, to perform localization using the update step of the Bayes filter on the real robot, is successfully satisfied. Overall, this lab was fun to design and but there were some minor problems faced during the experiments. It was extremely difficult to make the robot turn 360 degrees in one position again and it took a lot of trials before a good turn was obtained. To get better rotations, we frequently cleaned the robot wheels so there is better friction between the wheels and the floor. The lab guideline and the notebook were very clear and assisted a lot in programming the Bayes filter. The knowledge gained in this lab regarding the simulation environment will highly assist in Lab 13. 

---

## V. References

1. [ECE 5960 – Lab 12 Guideline](https://cei-lab.github.io/ECE4960-2022/Lab11.html)

#### Collaborators
Pillai, Nikhil

---

[Return to Main Page](https://spbarot.github.io/)
