##### 1.Cartographer官方文档

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

##### 3.安装Cartographer的依赖项

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

##### 4. 安装protobuf-cpp

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
##### 5. 安装absl-cpp


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

##### 6.安装其他库

尝试卸载当前版本并安装较旧版本的 markupsafe：

```C++

pip uninstall markupsafe

pip install markupsafe==2.0.1

```

日志中提到 Sphinx 1.8.5，但你可以尝试使用更高版本的 Sphinx 来看看是否能解决兼容性问题：

```C++

pip install --upgrade sphinx

```




##### 7.安装Cartographer

```C++
# 创建一个工作空间
mkdir -p ~/cartographer_ws/src
cd ~/cartographer_ws/src

# 克隆 Cartographer 源代码
git clone https://github.com/cartographer-project/cartographer.git

# 克隆 cartographer_ros 仓库
git clone https://github.com/cartographer-project/cartographer_ros.git

# 切换到 ROS 安装版本的标签（可选，避免版本不兼容）
cd ~/cartographer_ws
git checkout cartographer-ros-release-1.0.0

catkin_make_isolated --install --use-ninja
```



