#### 1.保存地图

```C++

rosservice call /finish_trajectory 0

rosservice call /write_state "{filename: '/home/ayl/map.pbstream', include_unfinished_submaps: true}"


```

#### 2.转化地图

```C++

rosrun cartographer_ros cartographer_pbstream_to_ros_map -map_filestem=/home/ayl/map -pbstream_filename=/home/ayl/map.pbstream -resolution=0.05

```
