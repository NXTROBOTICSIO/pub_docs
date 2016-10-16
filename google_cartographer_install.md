# Building and Instaling Google's Cartographer for ROS

### The below steps are a combination of instructional content from the following sources as well as our own experience:
  -  https://google-cartographer.readthedocs.io/en/latest/
  -  https://google-cartographer-ros.readthedocs.io/en/latest/

### 1. Install wstool and rosdep
```
sudo apt-get update
sudo apt-get install -y python-wstool python-rosdep ninja-build
```

### 2. Create a new workspace in 'catkin_ws'
```
mkdir catkin_ws
cd catkin_ws
wstool init src
```

### 3. Merge the cartographer_ros.rosinstall file and fetch code for dependencies
```
wstool merge -t src https://raw.githubusercontent.com/googlecartographer/cartographer_ros/master/cartographer_ros.rosinstall
wstool update -t src
```

### 4. Install deb dependencies
```
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y
```

### 5. Install dependencies
```
sudo apt-get install -y cmake g++ git google-mock libboost-all-dev libcairo2-dev libeigen3-dev libgflags-dev libgoogle-glog-dev  liblua5.2-dev libprotobuf-dev libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx
```
### 6. Install geometry2 (ROS packages for tracking coordinate transforms - includes tf2) 
```
git clone https://github.com/ros/geometry2.git ~/catkin_ws/src
```

### 7. Install libbullet development
```
sudo apt-get install libbullet-dev 
```

### 8. Install Ceres Solver
```
git clone https://ceres-solver.googlesource.com/ceres-solver
cd ceres-solver
mkdir build
cd build
cmake .. -G Ninja
ninja
ninja test
sudo ninja install
```
### 9. Build and install 
```
catkin_make_isolated --install --use-ninja
source install_isolated/setup.bash
```
**NOTE:**
When building, if you receive a variable error related to ***"CMAKE_MAKE_PROGRAM"*** and ***test_tf2***, you can temporarily resolve this by moving or deleting the catkin_ws/src/geometry2/test_tf2 directory and restarting the build.
```
mv ~/catkin_ws/src/geometry2/test_tf2/ ~/
```
## Additional Links ##
  -  [Turtlebot cartographer demo (gmapping 2d using depthimage_to_laserscan)](https://github.com/googlecartographer/cartographer_turtlebot/blob/master/cartographer_turtlebot/launch/demo_gmapping_depth_camera_2d.launch)

## Citations ##
  -  W. Hess, D. Kohler, H. Rapp, and D. Andor, Real-Time Loop Closure in 2D LIDAR SLAM, in Robotics and Automation (ICRA), 2016 IEEE International Conference on. IEEE, 2016. pp. 1271–1278.
