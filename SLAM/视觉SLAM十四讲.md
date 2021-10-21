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
**具体使用方法:[CSDN](https://blog.csdn.net/u011092188/article/details/77430988)**












