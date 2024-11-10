
# LSLiDAR-C16-3D-LiDAR

The C16 lidar realizes 360° three-dimensional high-speed scanning though 16
dense laser beams, with a detection distance of up to 200 m, a measurement
accuracy of ±3 cm, and a vertical angular resolution of 2°. It is widely used in
unmanned driving, automotive ADAS, smart transportation, service robot,
logistics, surveying and mapping, security, industry, ports and other fields.

![image](https://www.lslidar.com/wp-content/uploads/2022/07/C32-future-image.png)

## Installation

## Dependencies

1.ros

To run lidar driver in ROS environment, ROS related libraries need to be installed.

**Ubuntu 18.04**: ros-dashing-desktop-full

**Ubuntu 18.04**: ros-eloquent-desktop-full

**Ubuntu 20.04**: ros-foxy-desktop-full

**Ubuntu 20.04**: ros-galactic-desktop-full

**Ubuntu 22.04**: ros-humble-desktop-full

**Installation**: please refer to [http://wiki.ros.org](http://wiki.ros.org/)

2.ros dependencies

```bash
# install
sudo apt-get install ros-$ROS_DISTRO-pcl-ros ros-$ROS_DISTRO-pluginlib  ros-$ROS_DISTRO-pcl-conversions 
```

3.other dependencies

~~~bash
sudo apt-get install libpcap-dev
sudo apt-get install libboost${BOOST_VERSION}-dev   #Select the appropriate version
~~~
## Compile & Run

###  Clone

~~~bash
git clone https://github.com/CARVER-NEXT-GEN/LSLiDAR-C16-3D-LiDAR.git
~~~
###  Compile

~~~bash
cd LSLiDAR-C16-3D-LiDAR/Lslidar_C16_3D_ws/
colcon build
source install/setup.bash
~~~

### Run

run with single lidar:

~~~bash
ros2 launch lslidar_driver lslidar_cx_launch.py
ros2 launch lslidar_driver lslidar_cx_rviz_launch.py		#Automatically load rviz
~~~

run with double lidar:

~~~bash
ros2 launch lslidar_driver lslidar_double_launch.py
ros2 launch lslidar_driver lslidar_double_rviz_launch.py	#Automatically load rviz
~~~

run with four lidar:

~~~bash
ros2 launch lslidar_driver lslidar_four_launch.py
~~~
## Ethernet Setup

If you run the code above, it may not work; this could be due to an Ethernet setting. Which I will fix by setting the IP address of lidar to be the default as a factory configuration. 

### Step 1 Find Ethernet interface
~~~bash
sudo ifconfig
~~~
You will have something like this.

~~~bash
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:b7:8c:03:f6  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 169.254.19.84  netmask 255.255.0.0  broadcast 169.254.255.255
        ether 50:eb:f6:49:ca:74  txqueuelen 1000  (Ethernet)
        RX packets 2020  bytes 121200 (121.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1117  bytes 341576 (341.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 533350  bytes 1275090506 (1.2 GB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 533350  bytes 1275090506 (1.2 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlp3s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.9.154.244  netmask 255.255.252.0  broadcast 10.9.155.255
        inet6 fe80::a6d7:17e0:4532:c36f  prefixlen 64  scopeid 0x20<link>
        ether 34:6f:24:b6:a1:8f  txqueuelen 1000  (Ethernet)
        RX packets 1979455  bytes 2700198777 (2.7 GB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 382406  bytes 45157979 (45.1 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
~~~
So we identify Ethernet interface which is ```enp2s0```
As you can see, your LiDAR device is using the IP address 192.168.1.200 and attempting to communicate with 192.168.1.102. Currently, your Ethernet interface ```enp2s0``` has an IP address in the 169.254.x.x range, which is link-local and does not correspond to the same subnet as your LiDAR device.

### Step 2 Assign an IP Address to Your Ethernet Interface
Set your ```enp2s0``` interface to have the IP address 192.168.1.102 with a subnet mask of 255.255.255.0. This puts your computer and the LiDAR device on the same network subnet, allowing them to communicate.

~~~bash
sudo ifconfig enp2s0 192.168.1.102 netmask 255.255.255.0 up
~~~
- ```enp2s0``` is your Ethernet interface.
- 192.168.1.102 is the IP address you want to assign.
- netmask 255.255.255.0 sets the subnet mask.
- up brings the interface online.

### Step 3 Run Again (Example)
~~~bash
ros2 launch lslidar_driver lslidar_cx_rviz_launch.py
~~~

## Other ways
You can also use the following links to fix or set parameters.
 - [Lslidar_ROS2_driver](https://github.com/Lslidar/Lslidar_ROS2_driver)
 - [LSLIDAR_CX_V4.2.4_240410_ROS2](https://github.com/Lslidar/Lslidar_ROS2_driver/blob/C16_V4.0/README_en.md)
  - [LSLIDAR_C16](https://www.lslidar.com/product/c32-16-mechanical-lidar/)
