# ilab_agilex
ROS2 workspace for Agilex robots setup @ Innovlab

## Getting Started
***Required setup : Ubuntu 22.04 LTS***

1.  Install `ros2` packages. The current development is based of `ros2 humble`. Installation steps are described [here](https://docs.ros.org/en/humble/Installation.html).
2. Source your `ros2` environment:
    ```shell
    source /opt/ros/humble/setup.bash
    ```
    **NOTE**: The ros2 environment needs to be sources in every used terminal. If only one distribution of ros2 is used, it can be added to the `~/.bashrc` file.
3. Install `colcon` and its extensions :
    ```shell
    sudo apt install python3-colcon-common-extensions
     ```
3. Create a new ros2 workspace:
    ```shell
    mkdir ~/ros2_ws/src
    ```
4. Install libasio
    ```shell
    $ sudo apt-get install libasio-dev
    ```
5. Pull relevant packages, install dependencies, compile, and source the workspace by using:
    ```shell
    cd ~/ros2_ws
    git clone https://github.com/maberobotics/ilab_agilex.git src/ilab_agilex
    rosdep install --ignore-src --from-paths . -y -r
    colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release --symlink-install
    source install/setup.bash
    ```

## Basic usage for tracer robot

1. Setup CAN-To-USB adapter

* Enable gs_usb kernel module(If you have already added this module, you do not need to add it)
    ```
    $ sudo modprobe gs_usb
    ```
    
* Bringup can device 
   ```
   $ sudo ip link set can0 up type can bitrate 500000

   ```
   
* Testing command
    ```
    # receiving data from can0
    $ candump can0
    ```
3. Launch ROS nodes
 
* Start the base node for the Tracer robot

    ```
    $ ros2 launch tracer_base tracer_base.launch.py
    ```

* Then you can send command to the robot
    ```
    $ ros2 topic pub /cmd_vel geometry_msgs/msg/Twist "linear:
    x: 0.0
    y: 0.0
    z: 0.0
    angular:
    x: 0.0
    y: 0.0
    z: 0.0" 

    ```
**SAFETY PRECAUSION**: 

Always have your remote controller ready to take over the control whenever necessary. 

## Navigation stack
To start lidar run 
```shell
$ ros2 launch tracer_bringup open_rslidar.launch.py
```
**Note:** To get data from the lidar, the IP of the PC connected to the lidar needs to be `192.168.1.102`

To start cartographer run
```shell
$ ros2 launch tracer_bringup cartographer.launch.py
```

The map can then be saved using 
```shell
$ ros2 run nav2_map_server map_saver_cli -f map
```

To start navigation run
```shell
$ ros2 launch tracer_bringup navigation.launch.py
```
