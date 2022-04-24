---
title: Lab 8 – Stunts
---
# Author: Swapnil Barot (NetID: spb228)
---

[Return to Main Page](https://spbarot.github.io/)

## I. Objective

The primary objective of this lab is to combine Lab 6 (Closed Loop PID) and Lab 7 (Kalman Filter) to produce a fast stunt on the robot. Two fun stunts using closed loop control and open loop control are created and displayed below. 

---

## II. Materials/Software

1. 1x Fully assembled robot with Artemis, ToF sensors, IMU sensor, and motor drivers
2. 1x USB A to C Cable
3. 2x Li-ion 3.7V 650mAh (or more) Battery

---

## III. Procedure/Design/Results
#### Lab Tasks Overview 

The following tasks shall be accomplished as a part of this lab: 
* Closed Loop Stunt
* Open Loop, Repeatable Stunts 

---

#### Controlled Stunts (Open Loop and Closed Loop) 

The goal for controlled stunts (Task A) in this lab are the following: 
* Start from < 4m from the wall
* Drive Fast Forward 
* Upon reaching the sticky mat, do a flip
* Drive back in the same direction

First, open loop control was tried to perform the stunt. This was done to ensure that the car can actually perform a flop before implementing closed loop control. As seen in the code snippet below, this involved running the forward for 3 seconds, abruptly stopping at the motors (analog output = 255 for all pins) and going backward for 3 seconds. The video below demonstrates the stunt. 

```
void forward(){
  analogWrite(A0, 205);
  analogWrite(A1, 0);
  analogWrite(A14, 200);
  analogWrite(A15, 0);
}
void backward(){
  analogWrite(A0, 0);
  analogWrite(A1, 255);
  analogWrite(A14, 0);
  analogWrite(A15, 250);
}
void stop(){
  analogWrite(A0, 255);
  analogWrite(A1, 255);
  analogWrite(A14, 255);
  analogWrite(A15, 255);
}

void loop() {
  delay(5000);
  forward();
  delay(3000)
  stop();
  backward();
  delay(3000)
  stop();
}

```
<br>

<iframe width="560" height="315" src="https://www.youtube.com/embed/1StS8apQ1rM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>

Once the open loop controlled stunt is verified, the closed loop stunt is designed. Closed Loop control stunt consists of sensing the distance the distance from the wall using a ToF sensor. If the distance is more than 925 mm, the car shall move forward. Else, the car shall stop, perform a flip, and move backwards. The program is integrated with BLE to start, stop, and transmit data as needed. Below is the related software and the video demonstration. 

```

void stunt_func(){
  distanceSensor2.startRanging();
  distance = distanceSensor2.getDistance(); 
  distanceSensor2.clearInterrupt();
  distanceSensor2.stopRanging();
  if (distance > 925) {
    forward();
  }
  else {
    counter++;
    if (counter==1){
    stop();
    backward();
  }
    else{
      backward();
   }
  } 
 }
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/SpvhNd7bW58" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

#### Hot Wheels Stunt 
The motivation for the open loop stunt comes from Hot Wheels cars and track sets. Two ramps are created out of wooden blocks and foam. The car is meant to go forward at full speed, get on the first ramp, fly off the ramp, and land on the second ramp. This task was extremely difficult to carry out as the car kept drifting away from the ramp or not land on the second ramp properly. Approximately 5 hours were spent trying to perfect the stunt. 

<img src="../images/Lab 8/hotwheels.jpg" width="500" alt="image1" class="inline"/>

<iframe width="560" height="315" src="https://www.youtube.com/embed/kTHjQ0wkGlM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/gtQZkKfpIMI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/V9A9JLMGVGo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

#### Bloopers 

<iframe width="560" height="315" src="https://www.youtube.com/embed/-GQfIVtlC3A" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## IV. Conclusion

The objective of this lab, to perform closed loop and open loop stunts, is successfully satisfied.  There were minor issues faced during the lab. While doing the close loop stunt, it was noticed that the distance sensor readings could not keep up with the maximum car speed as the car would not be able to slow down quick enough and perform the flip. It is also extremely difficult to control the car’s position after the flip and making it go back in the same direction it came from. Overall, this was a very fun lab and the staff was very helpful with debugging issues as usual.  

---

## V. References

1. [ECE 5960 – Lab 8 Guideline](https://cei-lab.github.io/ECE4960-2022/Lab8.html)

#### Collaborators
Patel, Priyam
<br>
Pillai, Nikhil

---

[Return to Main Page](https://spbarot.github.io/)


