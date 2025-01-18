##### ROS安装
本节课程以在Ubuntu20.04上安装ROS-Noetic为例，说明下如何安装ros。

1.设置ros源
```C++
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

2.设置密钥
```C++
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

```

3.更新源
```C++
sudo apt update
```


4.安装
安装ROS-Noetic
这里安装的是基础桌面版，终端输入，
```C++
sudo apt install ros-noetic-desktop -y

```

5.设置ROS环境
把ROS的路径加入到环境变量中，方便以后打开终端的时候，可以找打ROS的运行环境，终端输入，
```C++
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
```
然后重新打开终端或者source一次刷新环境变量，终端输入，
```C++
source ~/.bashrc

```

6.验证
终端输入
```C++
roscore



```

