# 视觉SLAM十四讲笔记

## 三维空间刚体运动

### 旋转、平移变换

#### 1. 坐标旋转的表示形式：

1. 旋转矩阵(3X3)  Q  SO(3)  `Eigen::Matrix3d`
2. 旋转向量(3X1)  `Eigen::AngleAxisd`
3. 四元数(4X1)  `Eigen::Quaterniond`


### Eigen的用法

#### 1. Eigen库中各种形式的表示如下：

* 旋转矩阵（3X3）:`Eigen::Matrix3d`
* 旋转向量（3X1）:`Eigen::AngleAxisd`
* 四元数（4X1）:`Eigen::Quaterniond`
* 平移向量（3X1）:`Eigen::Vector3d`
* 变换矩阵（4X4）:`Eigen::Isometry3d`
  

  **对旋转向量（轴角）赋值的三大方法：**
* 1. 使用旋转的角度和旋转轴向量（此向量为单位向量）来初始化角轴

	```
	AngleAxisd V1(M_PI / 4, Vector3d(0, 0, 1));	//以（0,0,1）为旋转轴，旋转45度
	```

* 2. 使用旋转矩阵转旋转向量的方式
     
	* 1 使用旋转向量的fromRotationMatrix()函数来对旋转向量赋值（注意此方法为旋转向量独有,四元数没有）

	```
	AngleAxisd V2;
	V2.fromRotationMatrix(t_R);
	```
	* 2 直接使用旋转矩阵来对旋转向量赋值
	```
	AngleAxisd V3;
	V3 = t_R;
	```
	* 3 使用旋转矩阵来对旋转向量进行初始化

	```
	AngleAxisd V4(t_R);
	```
* 3. 使用四元数来对旋转向量进行赋值

	* 1 直接使用四元数来对旋转向量赋值
	```
	AngleAxisd V5;
	V5 = t_Q;
	```
	* 2 使用四元数来对旋转向量进行初始化

	```
	 AngleAxisd V6(t_Q);
	```













