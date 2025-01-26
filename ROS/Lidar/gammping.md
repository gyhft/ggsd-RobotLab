##### 1.新建工作空间/编译功能包

以建立名字为rplidar_ws为例，终端输入，

```C++
mkdir rplidar_ws
cd rplidar_ws

mkdir src
cd src
catkin_init_workspace
```

然后把解压的rplidar_ros功能包/rf2o_laser_odometry功能包/slam_gmapping功能包复制到rplidar_ws/src目录下，然后在rplidar_ws目录下，使用catkin_make进行编译，

```C++
cd ~/rplidar_ws

catkin_make

```

编译通过后，把工作空间的路径添加到.bashrc中，

```C++
sudo gedit ~/.bashrc

```

把以下内容复制到文件末尾，
```C++

source ~/rplidar_ws/devel/setup.bash --extend

```

##### 2.绑定雷达端口名称

打开终端，输入以下命令，把功能包下的rplidar.rules文件复制到/etc/udev/rules.d中，

```C++
cd ~/rplidar_ws/src/rplidar_ros/scripts

sudo cp rplidar.rules  /etc/udev/rules.d/

```

然后重新拔插雷达串口，终端输入 

```C++
ll /dev/rplidar
```
##### 3.测试雷达

保存后退出，重新打开一个终端，输入以下语句，打开雷达并且在rviz中显示，

```C++
#a1雷达
roslaunch rplidar_ros view_rplidar_a1.launch
#a2雷达
roslaunch rplidar_ros view_rplidar_a2m12.launch
#a3雷达
roslaunch rplidar_ros view_rplidar_a3.launch
#s2雷达/s2l雷达
roslaunch rplidar_ros view_rplidar_s2.launch
#c1雷达
roslaunch rplidar_ros view_rplidar_c1.launch
#s3雷达
roslaunch rplidar_ros view_rplidar_s3.launch

```
以上命令也可以拆解为两个命令，以激光雷达s2为例：

```C++
roslaunch rplidar_ros rplidar_s2.launch

rosrun rviz rviz -d /home/ayl/catkin/src/rplidar_ros/rviz/rplidar.rviz
```

##### 4.启动gammping建图

```C++
roslaunch rplidar_ros rplidar_s2.launch

roslaunch rf2o_laser_odometry rf2o_laser_odometry.launch

roslaunch rplidar_ros test_rplidar.launch

roslaunch rplidar_ros test_gmapping.launch


```



