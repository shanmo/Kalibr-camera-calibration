## Camera

The stereo calibration requires two cameras. As such, we need to modify the camera.launch in the `/opt/ros/indigo/share/pointgrey_camera_driver/launch/`, based on this [issue](https://github.com/ros-drivers/pointgrey_camera_driver/issues/8).

Additionally, instead of simply running ros pointgrey driver, we need to list the cameras first, and then change the id values in the launch file accordingly. 

## Synchronization - preliminary test

Using the default setup in the initial run leads to an error of `Cameras are not connected through mutual observations, please check the dataset. Maybe adjust the approx. sync. tolerance.`

We need to change the default values by setting `--approx-sync`. By trial and error, 0.04 works. However, ideally the synchronization delay should be 0 for both cameras. The images may not be properly synchronized even if the cameras are launched at the same time.  

Hardware synchronization should be used to get accurate results. 

## Command

kalibr_calibrate_cameras --models omni-radtan omni-radtan --target april_6x4.yaml --bag stereo.bag --topics /left_camera/image_raw /right_camera/image_raw --show-extraction --approx-sync 0.04
 
## Calibration

The calibration pattern is placed on the ground so that it is kept as flat as possible. The shutter speed and gain are adjusted, and the exposure is kept constant. 

From the `results-cam-stereo`, the distance between the two cameras is 0.35663474m, as in `baseline T_1_0`. This is approximately the value we use to construct our stereo rig. 

