#### rf2o_laser_odometry安装
1.Github
https://github.com/MAPIRlab/rf2o_laser_odometry,这个链接，要注意在rf2o_laser_odometry的下方有两个branches,一个是ros2，一个是ros1的，因为我们大部分做激光雷达建图都是用的ros1,所以点击选择ros1，然后点击右上角的Code的
Donload ZIP,下载完成，放到home的文件夹。
2.代码修复
在源码rf2o_laser_odometry/src/CLaserOdometry2DNode.cpp中第126行的上面添加下面这句tf_listener.waitForTransform("/base_footprint","/laser_link", ros::Time(), ros::Duration(5.0));：

```C++
bool CLaserOdometry2DNode::setLaserPoseFromTf()
{
  bool retrieved = false;

  // Set laser pose on the robot (through tF)
  // This allow estimation of the odometry with respect to the robot base reference system.
  tf::StampedTransform transform;
  transform.setIdentity();
  try
  {
    tf_listener.waitForTransform("/base_footprint","/laser_link", ros::Time(), ros::Duration(5.0));
    tf_listener.lookupTransform(base_frame_id, last_scan.header.frame_id, ros::Time(0), transform);
    retrieved = true;
  }
  catch (tf::TransformException &ex)
  {
    ROS_ERROR("%s",ex.what());
    ros::Duration(1.0).sleep();
    retrieved = false;
  }

  //TF:transform -> Eigen::Isometry3d

  const tf::Matrix3x3 &basis = transform.getBasis();
  Eigen::Matrix3d R;

  for(int r = 0; r < 3; r++)
    for(int c = 0; c < 3; c++)
      R(r,c) = basis[r][c];

  Pose3d laser_tf(R);

  const tf::Vector3 &t = transform.getOrigin();
  laser_tf.translation()(0) = t[0];
  laser_tf.translation()(1) = t[1];
  laser_tf.translation()(2) = t[2];

  setLaserPose(laser_tf);

  return retrieved;
}
```

rf2o_laser_odometry/src/CLaserOdometry2D.cpp这里，修改如下
```C++
//First level -> Filter (not downsampling);
    if (i == 0)
    {
      for (unsigned int u = 0; u < cols_i; u++)
      {
        const float dcenter = range_wf(u);

        //Inner pixels
        if ((u>1)&&(u<cols_i-2))
        {
          if (dcenter > 0.f)
          {
          	if (std::isfinite(dcenter) && dcenter > 0.f){
            		float sum = 0.f;
            		float weight = 0.f;

            		for (int l=-2; l<3; l++)
            		{
              			const float abs_dif = std::abs(range_wf(u+l)-dcenter);
              			if (abs_dif < max_range_dif)
              			{
                			const float aux_w = g_mask[2+l]*(max_range_dif - abs_dif);
                			weight += aux_w;
                			sum += aux_w*range_wf(u+l);
              			}
            		}
            	range[i](u) = sum/weight;
            	}
          }
          else
            range[i](u) = 0.f;
            
        }

        //Boundary
        else
        {
          if (dcenter > 0.f)
          {
          	if (std::isfinite(dcenter) && dcenter > 0.f){
            		float sum = 0.f;
            		float weight = 0.f;

            		for (int l=-2; l<3; l++)
            		{
              			const int indu = u+l;
              			if ((indu>=0)&&(indu<cols_i))
              			{
                			const float abs_dif = std::abs(range_wf(indu)-dcenter);
                			if (abs_dif < max_range_dif)
                			{
                  				const float aux_w = g_mask[2+l]*(max_range_dif - abs_dif);
                  				weight += aux_w;
                  				sum += aux_w*range_wf(indu);
                			}
              			}
            		}
            		range[i](u) = sum/weight;
            	}
          }
          else
            range[i](u) = 0.f;
        }
      }
    }

```


