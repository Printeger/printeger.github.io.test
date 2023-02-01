- [1. Calibration Method](#1-calibration-method)
  - [1.1 Overall Structure](#11-overall-structure)
  - [1.2 Hand-Eye Caliration](#12-hand-eye-caliration)
  - [1.3 Solve AX = XB](#13-solve-ax--xb)
  - [1.4 Dual Quaternion](#14-dual-quaternion)
- [2. Usage](#2-usage)
  - [2.1 Data Collect Procedure](#21-data-collect-procedure)
  - [2.2 Precision Indicators](#22-precision-indicators)
    - [2.2.1 RPE: relative pose error](#221-rpe-relative-pose-error)
    - [2.2.2 ATE: absolute trajectory error](#222-ate-absolute-trajectory-error)
  - [2.3 in \& out](#23-in--out)
- [3. Related Page](#3-related-page)


# 1. Calibration Method

## 1.1 Overall Structure

![](pic/5/1.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/1.png)

## 1.2 Hand-Eye Caliration

![](pic/5/5.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/5.png)

> Mainly based on hand-eye calibration method

![](pic/5/2.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/2.png)

![](pic/5/3.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/3.png)

> The extrinsic matrix is from IMU axis to Lidar axis.
>
> Lidar_pose = Extrinsic * IMU_pose * Extrinsic.inverse()

## 1.3 Solve AX = XB

The function performs the Hand-Eye calibration using various methods. 

- [ ] One approach consists in estimating the rotation then the translation (separable solutions).

- [ ] Separable solutions solving separately damages the integrity of the kinematics problem and leads to coupling errors in the calculation of rotation and translation.

> R. Tsai, R. Lenz A New Technique for Fully Autonomous and Efficient 3D Robotics Hand/EyeCalibration
>
> F. Park, B. Martin Robot Sensor Calibration: Solving AX = XB on the Euclidean Group
> 
> R. Horaud, F. Dornaika Hand-Eye Calibration

- [ ] Another approach consists in estimating simultaneously the rotation and the translation (simultaneous solutions).

- [ ] Simultaneous solutions handle rotation and translation simultaneously and describes pose transformation in a unified manner, reducing complexity and improving calibration accuracy.

> N. Andreff, R. Horaud, B. Espiau On-line Hand-Eye Calibration
>
>K. Daniilidis Hand-Eye Calibration Using Dual Quaternions

In this program, we use K. Daniilidis's Dual Quaternions method to solve AX=XB problem.

## 1.4 Dual Quaternion
- dual number: 

![](pic/5/6.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/6.png)

- dual quaternion:

![](pic/5/7.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/7.png)

![](pic/5/8.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/8.png)

    point b to point a :

    use quaternion:  pa = q * pb * q̄

    A line in space with direction la through a point pa to a line in space with direction lb through a point pb: 

    la = q * lb * q̄

Hand-Eye Transformation with Unit Dual Quaternions:

![](pic/5/9.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/9.png)

    ǎ denote the screw of a camera motion, and b̌  denote the screw of the hand motion.

![](pic/5/10.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/10.png)

Multiplying on the right with q.

![](pic/5/11.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/11.png)

Let a = (0, a) and a' = (0, a'), as well as b = (0, b), b' = (0, b').

![](pic/5/12.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/12.png)

SVD: 

![](pic/5/13.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/13.png)

results:

![](pic/5/14.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/14.png)

    v7, v8 is the last two right-singular vectors

- compare to two-steps method(Tsai method) and nonlinear method:

![](pic/5/15.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/15.png)

       Behavior of the dual quaternion (DUAL), the nonlinear (NLIN), and the two-step (SEPA) algorithms with variation in noise. The RMS rotation error is shown on the left; the RMS relative translation error is on the right.

![](pic/5/16.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/16.png)

    The RMS error in rotation (left) and the RMS relative error in translation (right) as a function of the number of hand and camera motions.

# 2. Usage
## 2.1 Data Collect Procedure

More than three motions in different positions and orientations are required, so that the equation is full rank and can be solved linearly.

- [ ] Therefore, the algorithm requires the trajectory needs to contain translations and rotations(like vehicle to travel in the shape of figure 8, as shown in Figure down below). 

- [ ] The point cloud and GNSS+INS data of the trajectory need to be recorded simultaneously.

- [ ] There should be rich features in the experimental environment, like buildings, and less moving objects, like moving vehicle, which will affect the result of Lidar odometry.

- [ ] Do not test in an environment surrounded by tall buildings, this will affect the accuracy of GNSS.

- [ ] Could record more than one sets of data for verification if necessary. 

![](pic/5/4.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/4.png)

## 2.2 Precision Indicators
> estimate pose: 
> 
>![](pic/5/17.png)
>![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/17.png)

> reference pose:
>
> ![](pic/5/18.png)
> ![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/18.png)

### 2.2.1 RPE: relative pose error
> i frame RPE:
> 
> ![](pic/5/19.png)
> ![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/19.png)

![](pic/5/20.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/20.png)

### 2.2.2 ATE: absolute trajectory error

![](pic/5/21.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/21.png)

![](pic/5/22.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/22.png)

![](pic/5/23.png)
![](https://github.com/Printeger/printeger.github.io/raw/main/_posts/pic/5/23.png)

## 2.3 in & out

Input:

```
// PointCloud
pcl::PointCloud<pointXYZ> point_in;
// IMU pose
std::vector<Eigen::Matrix4d> IMU_pose;
// PointCloud timestamps
std::vector<double> timestamps;
```
Output:

```
//Extrinsic Matrix: 
Eigen::Matrix4d calibratedTransformation;
/*
|  R T  |
|_ 0 1 _|
*/
```


# 3. Related Page

> module design
> 
https://docs.opencv.org/4.x/d9/d0c/group__calib3d.html#gaebfc1c9f7434196a374c382abf43439b

> past calibration result