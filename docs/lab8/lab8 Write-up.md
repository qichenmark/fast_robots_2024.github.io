# Lab 8 Stun (Task: **flip!**)
## Overview
* Goal: make the car drive forward to the wall, flip, and drive back. 
* Highlevel: 
  1. Stage 1: Use lab5 knowledge to drive the car forward and stop in front of the wall, plus lab6 knowledge to ensure it always drive in a perfect straight line regardless of the distance; Meanwhile, record the distance it drived until the wall.  
  2. Stage 2: Immediately set the motor input to -255 to make it flip. 
  3. Stage 3: After the flip, the car might not completely be in the right angle due to small differences in motors' force and wheels' friction, so use lab6 knowledge to rotate it back the correct one, and then drive it back the same length as recorded in step 1. 
* Technical Approach(Pseudocode): 

```
int stage;
stage = 1;

// stage 1
if (stage == 1)
{
    if (tof > setpoint)
    {
        run to wall with const u;
    }
    else
    {
        stage = 2;
        t2 = millis(); // record stage2 start time as t2
    }
}
else if (stage == 2)
{
    // stage 2 flip
    if (t2 - millis() < 1000)
    {
        large negative u;
    }
    else
    {
        stage = 3;
    }
}
else if (stage == 3)
{
    lab5 stop on setpoint;
}
else
{
    appendLog(-2, ); // represent errors occured
}
```
## Chanllenges to flip
* Insufficient power supply
  * The power supply of 3.7v battery is insufficient to make the car stop quickly enough to flip. 
  * My solution: cut the battery wire and soider another battery socket in parallel. This enable the car to use 2 batteries and get 7.4v voltage. 
    ![alt text](<with the second battery.jpg>)
  * Issue: 7.4v is too high for the motor powered by a dual-path motor chip, it tiggered protection function and reduced the voltage output. Unfortunately, there is no battery with a voltage smaller than 7.4v but higher than 3.7v available in the lab. I have to short circuit the second battery socket. 
    ![alt text](<with the second battery removed.jpg>)

* Center of weight
  * the car's weight center is so balanced that it's hard to flip it. My solutions to overcome this are:
  1. attach extra battery in the battery slot because the battery slot is closer to the black side, which can move the weight center. 
   ![alt text](<Battery as weight.jpg>)
  2. stick a rod of coins in front of car. 
   ![alt text](<The rod of coins - blue side.jpg>)
   ![alt text](<The rod of coins - black side.jpg>)
  3. In the stage 1, drive forward using the black side of the car since weight center is closer to this side. 
  * All of these helped me moved the center of weight closer to the front and black side of the car, thus when the black side is facing up, it requrie much less power supply to flip. 
  

## Showcase:
* Flip in the lab:
  * <video controls src="Flip in lab.mp4" title="flip in the lab"></video>
* Flip on rough ground:
  * <video controls src="Flip on rough ground.mp4" title="flip on rough ground"></video>


