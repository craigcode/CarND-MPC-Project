# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program - Term 2, Project 5

Model Predictive Control

This project implements Model Predictive Control to drive a car around a simulated track. 

See project Rubric: https://review.udacity.com/#!/rubrics/896/view

## Implementation

### The Model

The Model state references:
* x & y, the car co-ordinates
* psi, the orientaton angle
* v, velocity
* cte, cross-track error
* epsi, psi error

The Actuator outputs are:
* steering angle
* throttle

The state of the current timestep uses the following update equations, based on the previous timestep state.

![vehicle model update equations](./vehicle-model-update-equations.png)

(Image Source: Udacity SDC ND, Lesson 20.9 MPC)

### Timestep Length and Elapsed Duration (N & dt)

I began mimicking the timestep and elapsed duration values implemented in the solution to the Mind The Line
problem posed in MPC Lesson 8, namely N = 25 and dt = 0.05 however this proved erratic and the car drove 
almost immediately off the track. A series of decreasing value iterations led to N = 15 and dt = 0.1 as the 
first combination I got to traverse the entire track. Tuning N = 10 and dt = 0.1 led to a smoother 
lap, holding the racing line. 

### Polynomial Fitting and MPC Preprocessing

The waypoint co-ordinates from the simulator were converted to the vehicle's co-ordinate system and a polyfit()
function used to plot the desired route to travel. The cross-track error is then calculated using a polynomial function, polyeval() and the orientation error from the derivative of the polynomial fit line. 


### Model Predictive Control with Latency

The application onMessage() event mimics real-world latency by introducing a sleep_for() delay of 100ms.
Vehicle model update equations referenced above are used to predict where the car will be after 100ms and this
predicted state together with the polyfitted co-efficients are passed to the solve method 
- mpc.Solve(state, coeffs) .


---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.

* **Ipopt and CppAD:** Please refer to [this document](https://github.com/udacity/CarND-MPC-Project/blob/master/install_Ipopt_CppAD.md) for installation instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

## Tips

1. It's recommended to test the MPC on basic examples to see if your implementation behaves as desired. One possible example
is the vehicle starting offset of a straight line (reference). If the MPC implementation is correct, after some number of timesteps
(not too many) it should find and track the reference line.
2. The `lake_track_waypoints.csv` file has the waypoints of the lake track. You could use this to fit polynomials and points and see of how well your model tracks curve. NOTE: This file might be not completely in sync with the simulator so your solution should NOT depend on it.
3. For visualization this C++ [matplotlib wrapper](https://github.com/lava/matplotlib-cpp) could be helpful.)
4.  Tips for setting up your environment are available [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)
5. **VM Latency:** Some students have reported differences in behavior using VM's ostensibly a result of latency.  Please let us know if issues arise as a result of a VM environment.



## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/b1ff3be0-c904-438e-aad3-2b5379f0e0c3/concepts/1a2255a0-e23c-44cf-8d41-39b8a3c8264a)
for instructions and the project rubric.
