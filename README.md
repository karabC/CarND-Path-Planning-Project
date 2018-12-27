# CarND-Path-Planning-Project
This is a Term-3 Project for the Self_driving Car Engineer Nanodegree.
The working project repo is from https://github.com/karabC/CarND-Path-Planning-Project

### Goals
In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.

## Model
### State 
In this project, there are few main finite states "Keep in the lane", "change lane (Left)", "Change lane (Right)" and "Decelerate", "Accelerate (High acceleration 7.5m/s^2 )", "Middle Accelerate (5m/s^2)", "Mild Accelerate (2.5m/s^2)", "Low Accelerate(1.25m/s^2)".

There are two major action parameters for compositing above state, *Target Lane* and *Target Velocity*.

### Change of State
To determine whether the state change, we use information for the policy.
1. Too Close with front car with same lane
2. Possible to change other lane, which is without vehicle for a safe threshold range
3. Any relatively slow car in the targeted lane to switch. To avoid crash as the deceleration might not be enough.

#### Accelerate States
The reason for including different level of acceleration is for stability. For higher speed, the environment changed relatively fast and gentle acceleration can keep the car drive more robust. Of course, it would take longer time to achieve the targeted velocity (45MPH, tunable, but 45MPH is indeed a good choice to prevent violating speed during switching lane)

## Smoothening
Thanks Aaron for the introducing the usage of previous path point for a much more stable suggested path. However, the suggested 50 points are still not robust enough. After several trial, 100 points are good for keeping a good result.

## Cold Start
The car acclerates from static. As mentioned, graduated acceleration was applied for different level of velocity for the sake of balancing the speed and stability.


## Furture Refinement
There are still plenty of room to refine the highway path planning. I made several assumption on the ideal environment. More robust rules should be built to cater some unflavor cases, such as sudden lane switching.

It would be also a good trial to apply cost function in this project.