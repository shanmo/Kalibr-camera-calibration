## Calibration

To keep the calibration pattern as flat as possible, we put it against a wall. As for the illumination condition and how to move the camera imu system, the example dataset is a good reference. 

The baseline of the stereo camera is about 0.357m, and the IMU is placed in the middle of the two cameras. The camera frame rate is 20 Hz, and the frequency of IMU is 160 Hz. 

The autopilot sends a triggering signal to the two cameras, and records the timestamp and sequence number. Then the camera captures the image and records the sequence number.
In this way, we can match the timestamp of the autopilot and cameras by checking the sequence number. The imu timestamp is the same with the autopilot. 

For the camera, we set the exposure to 1.35, shutter speed is 0.01, gain is 10. The shutter speed is kept small to eliminate motion blur. 

If we do not enable --time-calibration, the reprojection error is a bit large, but the T_ci is quite accurate, since the translation for both cameras to imu is 0.18m. 

If we calibrate with --time-calibration, the  reprojection error is quite small (the reprojection error should be smaller than 1 pixel), however the T_ci are slightly different. 
According to this [post](https://groups.google.com/forum/#!topic/kalibr-users/m17VpekSHtg), it is a good idea to enable time sync. 

## Note

The IMU frequency can be set in imu.yaml. Remember to change it if whenever the IMU frequency changes! 

If the shutter speed is changed, we need to re-calibrate the stereo camera. In fact, instead of using the camchain.yaml obtained previously, it seems better to collect rosbag of camera and imu data, then run kalibr_calibrate_cameras
to do camera calibration, followed by kalibr_calibrate_imu_camera, since the camera settings could be changed.  

## IMU parameters

We are using MPU 6050, and [here](https://groups.google.com/forum/#!topic/kalibr-users/rV3vUa8d228) is a discussion about its noise model.
According to the discussion, time-sync between image and IMU data is very critical. We may also have to increase the noise value obtained from Allan variance estimates.

The symbols representing different parameters can be found in the documentation of OKVIS [here]( http://ethz-asl.github.io/okvis_ros/structokvis_1_1ImuParameters.html#aff5d46f11494e24bffd0c56ddfe877f6).

Based on the OKVIS code, Accelerometer saturation is a_max, and Gyroscope saturation is g_max. These values are defined as the full-scale range
in the dataset. For instance, we are using MPU6050, according to the datasheet: 

```
gyroscope full-scale range of ±250, ±500, ±1000, and ±2000°/sec (dps)
accelerometer full-scale range of ±2g, ±4g, ±8g, and ±16g
```

These values are user-programmable.

Sometimes drift noise density is used in lieu of random walk.

A good post about IMU could be found [here](https://zhuanlan.zhihu.com/p/20082486). Another one is the [git book](http://www.crazepony.com/book/index.html).

## Transformation 

The transformation in the result is expressed in the matrix form. It can be converted to quaternion form online [here](http://www.euclideanspace.com/maths/geometry/rotations/conversions/matrixToQuaternion/).


