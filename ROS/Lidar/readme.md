1. 桌面home那里创建一个catkin文件夹，然后里面创建src文件夹

在这个github代码仓，https://github.com/Slamtec/rplidar_ros/tree/dev-ros1，下载之后，解压到创建的catkin的src文件夹中(改名，rplidar_ros-ros1改为rplidar_ros)

2. 返回到catkin文件夹，打开terminal，然后输入命令

```C++
catkin_make
source ~/catkin/devel/setup.bash

```


3.返回到catkin，然后打开terminal,运行节点
```C++
roslaunch rplidar_ros rplidar_s2.launch


```
4.显示点云图

```C++

rosrun rviz rviz -d /home/ayl/catkin/src/rplidar_ros/rviz/rplidar.rviz


```
