## Camera

We are trying to calibrate fisheye lenses (Lensation GmbH BF2M2020S23) mounted on pointgrey cameras (CM3-U3-31S4C-CS). We are calibrating a single camera for a preliminary test, we want to calibrate two cameras and one imu eventually.

The fisheye lenses are mounted on the camera base by using LRM12V2, LRICM and AD04M. The assembly images are contained in this folder as well. 
Note that ACC-01-5004, ER-5, ext ring 5mm CS-C Mount Adapter and LROCM are not used. 

We use `kalibr_camera_focus --topic [IMAGE_TOPIC]` to adjust the camera focus, and a decent result would be about -13. 

## Image collection

First we need to configure the camera. We need to:

1. Lower the shutter speed to avoid motion blur. The shutter speed should be about 0.1. 
2. Decrease the gain to reduce the noise in the image. The gain should be about 10. 
3. Keep constant exposure time as mentioned in this [issue](https://github.com/ethz-asl/kalibr/issues/40). 
4. Set the frame rate to 4 in order to reduce the redundancy. 

Since we are using PointGrey camera, we can set the parameters using the following commands. Our OS is Ubuntu 14.04.4, with ROS Indigo. 

```
roscore
roslaunch pointgrey_camera_driver camera.launch
rosrun rqt_reconfigure rqt_reconfigure
```

We can also set the parameters in `/opt/ros/indigo/share/pointgrey_camera_driver/launch/camera.launch`. 

We can open `rqt` to view the image. To record rosbag, use `rosbag record /camera/image_raw`. The rosbag will be recorded in the current directory, and its filename is the present time.

We can also use Flycapture to record the image sequence and then use the `kalibr_bagcreater --folder dataset-dir --output-bag awsome.bag` to convert it into rosbag. 
However, we cannot direclty use the images as their filenames are not consistent with the format that kalibr_bagcreater recognizes. The problem is with `timestamp = rospy.Time( secs=int(timestamp_nsecs[0:-9]), nsecs=int(timestamp_nsecs[-9:]) )` in `loadImageToRosMsg`.
Although the trace back appears to be some python libary is missing, but the real issue is that the timestamp could not be created. 

We may want to downsample the image size. In that case, we can record a rosbag, and then use `kalibr_bagextractor` to extract the frames from the rosbag. The filenames are consistent with the format used by kalibr_bagcreater. 
After we downsample the images, we can use kalibr_bagcreater to convert them back to rosbag. 

## Calibration

Because we are calibrating fisheye lenses, we use the `omni-radtan` camera model. 

However, when we use the omni-radtan camera model provided in Kalibr, we get an error, which says xi < 0, but xi should be positive. Later we got a successful calibration by changing the orientation of the calibration pattern from landscape to portrait.

We can use `--show-extraction` to display the target extraction. If the target is not properly detected, a warning will be shown and that frame will be discarded. 



## Validation

We can use `kalibr_camera_validator --cam camchain.yaml --target target.yaml` to validate the calibration results contained in camchain.yaml. 
