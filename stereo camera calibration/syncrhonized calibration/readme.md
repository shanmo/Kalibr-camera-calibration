## Calibration

To keep the calibration pattern as flat as possible, we put it against a wall. As for the illumination condition and how to move the camera imu system, the example dataset is a good reference. 

The camera frame rate is 20 Hz, and the frequency of IMU is 160 Hz. The baseline of the stereo camera is about 0.357m, and the IMU is placed in the middle of the two cameras.

For the camera, we set the exposure to 1.35, shutter speed is 0.01, gain is 10. The shutter speed is kept small to eliminate motion blur. 

If we do not enable --time-calibration, the reprojection error is a bit large, but the T_ci is quite accurate, since the translation for both cameras to imu is 0.18m. 

If we calibrate with --time-calibration, the  reprojection error is quite small, however the T_ci are slightly different. 

## Note

The IMU frequency can be set in imu.yaml. Remember to change it if whenever the IMU frequency changes! 

If the shutter speed is changed, we need to re-calibrate the stereo camera. In fact, instead of using the camchain.yaml obtained previously, it seems better to collect rosbag of camera and imu data, then run kalibr_calibrate_cameras
to do camera calibration, followed by kalibr_calibrate_imu_camera, since the camera settings could be changed.  



