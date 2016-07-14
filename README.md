## Obstacle Avoidance using stereo RGB cameras

This is a ROS package for local obstacle avoidance on the Clearpath Jackal. It performs local 3D reconstruction and then generates an obstacle scan and publishes it as a `sensor_msgs/LaserScan` message. Using the laser scan data, the robot avoids obstacles locally.

### Dependencies

- ROS
- OpenCV
- libelas

### Starting the cameras

Currently using two Logitech C920s as a stereo pair. SSH into the Jackal, and start the webcams using the following commands.

```bash
sudo chmod a+rw /dev/video0
roslaunch jackal_nav webcam_left.launch
```

```bash
sudo chmod a+rw /dev/video1
roslaunch jackal_nav webcam_right.launch
```

### Safe navigation

Once the camera topics are being published, generate a point cloud and an obstacle scan using this command.

```bash
rosrun jackal_nav point_cloud [path/to/calibration/file.yml]
```

Subscribes to two camera topics: 

- `/webcam_left/image_raw/compressed`
- `/webcam_right/image_raw/compressed`

Publishes three topics: 

- `/webcam_left/depth_map`
- `/webcam_left/point_cloud`
- `/webcam_left/obstacle_scan`

Now the run the `navigate` node for safe navigation. 

```bash
rosrun jackal_nav navigate
```

### Drive modes

Use the following combos on the DualShock controller to switch between modes

- `R1 + R2`: Use left stick to drive. The jackal stops in front of obstacles.
- `Hold X`: Let the Jackal move in the direction it's facing avoiding obstacles in its way.

### Disparity Maps

Dense disparity maps are generated using [libelas](http://www.cvlibs.net/software/libelas/) which is a great open source library for realtime generation of disparity maps.
