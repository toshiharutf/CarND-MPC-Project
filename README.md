# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---

## Description
In this project, a self driving car implementation was done with an MPC controller. The MPC controller controls both the steering and throttle, so that the car follows the path trajectory.
The optimal NLP solver used was Ipopt, with CppAD to provide the Jacobian and Hessian. 

The Udacity simulator provide as input the way points of the roadway center. This curve is then approximated using a third degree polynomial. However, these points are provided in world's (or simulator map's) coordinate system, so they have to be converted first. The code for this part is in main.cpp, lines 104-116. Also in the main.cpp file, lines 124-129, the initial states values are set up and send to the MPC controller.

MPC is an implementation of an optimal control problem. In an optimization problem, a cost function is minimized within the system's (vehicle model) constrainsts. For this project, the cost function has mainly the deviation from the road waypoints (MPC.cpp 61-63). To maximize the speed of the vehicle, a cruise speed is setup in fed to the cost function, also. To reduce jerkiness in the actuators, their variables are also introduced (MPC.cpp 65-68). 

The Udacity car simulator introduces an artificial **actuator delay** for the steering and throttle of 0.1 s. This delay has been taken in account by modelling both actuators as a **second degree linear system**. The steering model uses variables delta_x1 and delta_x2. The output for this model is delta_x1, and its input, delta. This is similar for the throttle system model. Both actuator models increases the system states number to N=10. However, the Ipopt solver is still able to solve the problem on time to control the car. The implementation of both models can be seen in lines MPC.cpp 145-150. Also notices that, if you don't take into account this delay, the system is very unstable. 

I also noticed that the Udacity car simulates some kind of **aerodynamic drag**, since the car accelerates slower when the speed increases. In line 142, the C_drag*v² factor was introduced, so that the throttle increases to compensate this effect. I noticed a small increase in speed, without compromising the steering and braking response in curves.

## Conclusions
The MPC controller works quite well, as it can be seen in the following [video](https://youtu.be/XX6yrSNat6w). In one part of the track, it almost reach 100 mph.

## Improvements
It would be cool to know the exact model the Udacity simulator uses. The MPC controller is as good as the model used, after all.

## Netbeans IDE
I used Netbeans for this project. You may find the nbproject file inside.

## Real implementation of an MPC controller for selfdriving cars
I implemented an MPC controller on  RC 1:5 car. You can wath the results in the following video.

[Single track - dynamic model MPC] (https://youtu.be/lzwB4FAgJ9Y)

If you want to know more about MPC, vehicle models, and how the algorythm in the video was implemented, you can read my master's thesis [here](http://tesis.pucp.edu.pe/repositorio/handle/123456789/8901).

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



