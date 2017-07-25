# Unscented Kalman Filter Project
[Self-Driving Car Engineer Nanodegree Program]
---

In this project utilize an Unscented Kalman Filter to estimate the state of a moving object of interest with noisy lidar and radar measurements. Passing the project requires obtaining RMSE values that are lower that the tolerance specified for position px, py and velocities vx, vy in 2D dimension. 

**Requirements**: RMSE of state vectors should be less than [.09, .10, .40, .30] for Dataset 1 and [0.20, 0.20, 0.55, 0.55] for Dataset 2.

[unscented]: ./images/unscented.png
[dataframe]: ./images/dataframe.png
[ds1]: ./images/ds1.png
[ds2]: ./images/ds2.png
[estimate_process_noise]: ./images/estimate_process_noise.png
[NIS]: ./images/NIS.png

## Setup 

This project involves the Term 2 Simulator which can be downloaded [here](https://github.com/udacity/self-driving-car-sim/releases). This repository includes two files that can be used to set up and intall [uWebSocketIO](https://github.com/uWebSockets/uWebSockets) for either Linux or Mac systems.

Once the install for uWebSocketIO is complete, the main program can be built and ran by doing the following from the project top directory.

Build
1. mkdir build && cd build
2. cmake .. && make
3. ./ExtendedKF ../data/sample-laser-radar-measurement-data-1.txt output1.txt > input1.log

Delete old build before rebuild again
1. cd ..
2. rm -r build

## Theory

Similar to Extended Kalman filter, Unscented Kalman filter also needs to solve the challenge of non-linear equation when projecting radar measurement into the state system. This challenge becomes harder when the motion system is not linear, for example in the case where the object is turning in a circular motion.

Enhanced from Extended Kalman filter which uses Jacobian matrix to linearize radar measurement equation, Unscented Kalman filter deal with this non-linearity through sigma points. In particular, to calculate the distribution of an output when feeding a Gaussian input through non-linear systems, the Unscented method samples some *sigma points* from the distribution, calculates the sampled outputs individually and then estimates the output distribution through the results. The following picture illustrates the process

![alt text][unscented]

## Implementation

We need to tune the process noise parameters *std_a* and *std_yawdd* in order to get your solution working on both datasets. This is done by observing the variance of longitudinal acceleartion and yaw acceleration across the whole process. The Python notebook *plot.py* in this folder does that. By outputing all the necessary values to a log file (result_ds1.txt, result_ds1.txt), we have the following dataframe

![alt text][dataframe]

![alt text][estimate_process_noise]

From the histograms, we can guess the values should be around $std_a = 0.4$ and $std_yawdd = 0.6$. The NIS graphs for the two sensor types are used to confirm the consistency of the Unscented Kalman filter

![alt text][NIS]

## Result

The position tracking for the objects in Dataset 1 and Dataset 2 are plotted from the Simulator. They meet the required criteria specified at the beginning.

![alt text][ds1]

![alt text][ds2]

