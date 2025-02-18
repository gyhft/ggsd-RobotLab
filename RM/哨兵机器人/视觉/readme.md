#### Ubuntu 20.04 安装 Ros2(Foxy版本）

1. 设置软件源

打开终端，执行以下命令以允许 apt 使用 HTTPS 源：

```C++

sudo apt update && sudo apt install curl gnupg2 lsb-release

```

添加 ROS 2 apt 仓库的 GPG 密钥：(要先开代理）

```C++

curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

```

添加 ROS 2 软件源：

```C++

sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'

```

2. 安装 ROS 2

更新 apt 包索引：

```C++
sudo apt update
```

安装 ROS 2 Foxy 的桌面完整版，该版本包含 ROS 2 核心、RViz、命令行工具等：

```C++

sudo apt install ros-foxy-desktop

```

3. 设置环境变量
   
每次打开终端自动设置环境变量，可将上述命令添加到 ~/.bashrc 文件中：

```C++

echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
source ~/.bashrc

```

4. 安装依赖工具

ROS 2 使用 rosdep 来安装系统依赖项，执行以下命令初始化 rosdep：

```C++
sudo apt install python3-rosdep
sudo rosdep init
rosdep update

```

5. 验证安装

安装完成后，可以通过运行一个简单的示例来验证 ROS 2 是否正常工作。打开一个新的终端，运行以下命令启动一个 talker 节点：

```C++

ros2 run demo_nodes_cpp talker

```

再打开另一个新的终端，运行以下命令启动一个 listener 节点：

```C++

ros2 run demo_nodes_py listener

```

如果看到 talker 节点不断发布消息，而 listener 节点能够接收到这些消息，说明 ROS 2 已经成功安装并正常工作。
