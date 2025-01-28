#### 1.Cartographer官方文档

在这个网址，https://google-cartographer.readthedocs.io/en/latest/

System Requirements

尽管 Cartographer 可能在其他系统上运行，但已经确认在满足以下要求的系统上可以正常工作：

* 64-bit,modern CPU
* 16 GB RAM
* Ubuntu 18.04/20.04/22.04
* gcc version 7.5.0,8.3.0,9.3.0,10.2.1,11.2.0

上面这个gcc就很重要的，我的电脑是Ubuntu 20.04,但gcc是9.4.0，所以要源码编译安装9.3.0的gcc.

#### 2.源码安装gcc 9.3.0

wget https://ftp.gnu.org/gnu/gcc/gcc-9.3.0/gcc-9.3.0.tar.gz


解压源码包

```C++
tar -xvf gcc-9.3.0.tar.gz
cd gcc-9.3.0


```

创建构建目录

```C++
mkdir build
cd build


```

配置GCC编译选项

```C++
../configure --prefix=/usr/local/gcc-9.3.0 --enable-languages=c,c++ --disable-multilib

```

在build目录下打开terminal,编译GCC

```C++
make -j$(nproc)
sudo make install


```

更新环境变量

```C++
sudo gedit ~/.bashrc

export PATH=/usr/local/gcc-9.3.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/gcc-9.3.0/lib64:$LD_LIBRARY_PATH

```

验证GCC:

```C++
gcc --version


```

#### 3.安装Cartographer的依赖项

```C++

# 安装 Cartographer 所需的依赖项

sudo apt install -y libboost-iostreams-dev
sudo apt install -y  libgtest-dev
sudo apt install -y  libprotobuf-dev 
sudo apt install -y  protobuf-compiler 
sudo apt install -y  libeigen3-dev 
sudo apt install -y  libsuitesparse-dev 
sudo apt install -y  libceres-dev 
sudo apt install -y  liblua5.3-dev 
sudo apt install -y  libgoogle-glog-dev 
sudo apt install -y  libpcl-dev 
sudo apt install -y  python3-pip 
sudo apt install -y  libyaml-cpp-dev 
sudo apt install -y  libgflags-dev

```

#### 4. 安装protobuf-cpp

```C++
到这个链接找到16，就是3.4.1的版本

https://github.com/protocolbuffers/protobuf/releases

点击下载protobuf-cpp-3.4.1.tar.gz

解压之后，放到cartographer这个文件夹里面(改名为protobuf)

然后打开终端：

输入
cd protobuf
sudo ./autogen.sh

在执行 ./configure 之前，确保传递 -fPIE 编译选项。你可以通过设置环境变量来确保这一点：

export CXXFLAGS="-fPIE $CXXFLAGS"
export LDFLAGS="-pie $LDFLAGS"


sudo ./configure --prefix=$INSTALL_DIR

sudo make

sudo make install

```
#### 5. 安装absl-cpp


```C++

git clone https://github.com/abseil/abseil-cpp.git

cd abseil-cpp

mkdir build

cd build

cmake -DCMAKE_POSITION_INDEPENDENT_CODE=ON ..

cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local

make -j$(nproc)

sudo make install

```

#### 6.安装其他库

尝试卸载当前版本并安装较旧版本的 markupsafe：

```C++

pip uninstall markupsafe

pip install markupsafe==2.0.1

```

日志中提到 Sphinx 1.8.5，但你可以尝试使用更高版本的 Sphinx 来看看是否能解决兼容性问题：

```C++

pip install --upgrade sphinx

```




#### 7.安装Cartographer

安装辅助工具库:

```C++

sudo apt-get install -y python3-wstool python3-rosdep ninja-build stow

```
开始

```C++
mkdir cartographer_ws

cd cartographer_ws

wstool init src
 
wstool merge -t src https://raw.githubusercontent.com/cartographer-project/cartographer_ros/master/cartographer_ros.rosinstall
 
wstool update -t src

```

rosdep相关

```C++
sudo rosdep init

rosdep update

rosdep install --from-paths src --ignore-src --rosdistro=${noetic} -y

```
如果最后一步碰上报错：

ERROR: the following packages/stacks could not have their rosdep keys resolved to system dependencies: cartographer: [libabsl-dev] defined as "not available" for OS version [focal]


那也很好解决。把cartographer_ws/src/cartographer文件夹中的package.xml 文件中的第46行<depend>libabsl-dev</depend>删掉就完事儿了

之后运行下面的代码：

```C++

rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y

```

最后编译：

```C++

catkin_make_isolated --install --use-ninja

```

source一下：

```C++

sudo gedit ~/.bashrc

```
把这个语句加到文件里面，然后保存：

```C++

source ~/cartographer/devel_isolated/setup.bash

```

#### 8.Cartographer联合激光雷达调试

修改revo_lds.lua

```C++
sudo gedit  ~/cartographer/src/cartographer_ros/cartographer_ros/configuration_files/revo_lds.lua

```
修改为如下：

```C++
include "map_builder.lua"
include "trajectory_builder.lua"

options = {
  map_builder = MAP_BUILDER,
  trajectory_builder = TRAJECTORY_BUILDER,
  map_frame = "map",  -- 地图坐标框架
  tracking_frame = "laser",  -- 激光扫描的坐标框架
  published_frame = "laser",  -- 发布的坐标框架
  odom_frame = "odom",  -- 里程计坐标框架
  provide_odom_frame = true,  -- 提供里程计坐标框架
  publish_frame_projected_to_2d = false,  -- 不发布投影到2D的帧
  use_pose_extrapolator = true,  -- 使用位姿外推器
  use_odometry = false,  -- 启用里程计数据
  use_nav_sat = false,  -- 禁用导航卫星
  use_landmarks = false,  -- 禁用地标
  num_laser_scans = 1,  -- 单个激光扫描
  num_multi_echo_laser_scans = 0,  -- 禁用多回波激光扫描
  num_subdivisions_per_laser_scan = 1,  -- 每个激光扫描的子划分数
  num_point_clouds = 0,  -- 禁用点云数据
  lookup_transform_timeout_sec = 0.2,  -- 查找坐标变换的超时
  submap_publish_period_sec = 0.1,  -- 加快子地图发布周期
  pose_publish_period_sec = 5e-3,  -- 位姿数据发布周期
  trajectory_publish_period_sec = 30e-3,  -- 轨迹发布周期
  rangefinder_sampling_ratio = 1.,  -- 激光雷达采样比率
  odometry_sampling_ratio = 1.,  -- 里程计采样比率
  fixed_frame_pose_sampling_ratio = 1.,  -- 固定帧位姿采样比率
  imu_sampling_ratio = 1.,  -- IMU采样比率
  landmarks_sampling_ratio = 1.,  -- 地标采样比率
}

MAP_BUILDER.use_trajectory_builder_2d = true  -- 使用2D轨迹构建器

TRAJECTORY_BUILDER_2D.submaps.num_range_data = 35  -- 每个子地图包含的最大激光数据点数
TRAJECTORY_BUILDER_2D.min_range = 0.3  -- 激光雷达的最小探测范围
TRAJECTORY_BUILDER_2D.max_range = 8.0  -- 激光雷达的最大探测范围
TRAJECTORY_BUILDER_2D.missing_data_ray_length = 1.0  -- 数据缺失的最大距离
TRAJECTORY_BUILDER_2D.use_imu_data = false  -- 禁用IMU数据
TRAJECTORY_BUILDER_2D.use_online_correlative_scan_matching = true  -- 启用在线相关扫描匹配
TRAJECTORY_BUILDER_2D.real_time_correlative_scan_matcher.linear_search_window = 0.1  -- 调整线性搜索窗口
TRAJECTORY_BUILDER_2D.real_time_correlative_scan_matcher.translation_delta_cost_weight = 10.0  -- 平移匹配权重
TRAJECTORY_BUILDER_2D.real_time_correlative_scan_matcher.rotation_delta_cost_weight = 1e-1  -- 旋转匹配权重

POSE_GRAPH.optimization_problem.huber_scale = 1e2  -- 优化问题中的Huber缩放因子
POSE_GRAPH.optimize_every_n_nodes = 35  -- 每35个节点进行一次图优化
POSE_GRAPH.constraint_builder.min_score = 0.65  -- 最小约束得分

return options

```

修改demo_revo_lds.launch

```C++
sudo gedit  ~/cartographer/src/cartographer_ros/cartographer_ros/launch/demo_revo_lds.launch

```
修改为如下：

```C++
<launch>
  <param name="/use_sim_time" value="true" />
 
  <node name="cartographer_node" pkg="cartographer_ros"
      type="cartographer_node" args="
          -configuration_directory $(find cartographer_ros)/configuration_files
          -configuration_basename revo_lds.lua"
      output="screen">
    <remap from="scan" to="scan" />
  </node>
 
  <node name="rviz" pkg="rviz" type="rviz" required="true"
      args="-d $(find cartographer_ros)/configuration_files/demo_2d.rviz" />
  
</launch>

```
以上修改完成后，要重新编译,在Cartographer里面打开Terminal，输入如下命令：

```C++

catkin_make_isolated --install --use-ninja

```

最后运行命令：

在rplidar_ws里面打开terminal:

```C++

roslaunch rplidar_ros rplidar_s2.launch

```
在cartographer里面打开terminal:

```C++

roslaunch cartographer_ros demo_revo_lds.launch

```



