## linux操作
### 命令
* `pwd` : 查看当前默认路径
* `cd` : 切换路径
  * `cd <路径>` : 下级目录
  * `cd ..` : 上级目录
* `mkdir <文件夹名>` : 创建文件夹
* `ls` : 文件列表
* `touch <文件名>` : 创建文件
* `mv <文件名> <目标路径>` : 移动文件
* `cp <文件名> <目标路径>` : 复制文件
* `rm <文件名>` : 删除文件
* `rm -r <目标路径>` : 删除文件夹
* `sudo` : 提高用户权限

## ROS 机器人操作系统
**ROS = 通讯机制 + 开发工具 + 应用功能 + 生态系统**   
**使命：提高机器人研发中的复用率**

### ROS中的开发工具
* 命令行&编译器
* TF坐标变换
* Riz
* QT工具箱
* Gazebo

### ROS中的应用功能
* Navigatuin
* SLAM
* Movelt

### 常用指令
* `rosnode list` 查看节点列表
* `rostopic pub -r 10 /<话题名>` 发布话题信息
* `rosservice call /服务文件 “变量:val”` 发布服务请求
* `rosbag record -a -O <fileName>` 话题记录
* `rosbag play <fileName>` 话题复现

### 创建工作空间与功能包
**工作空间结构：**
* src : 代码空间
* build ： 编译空间
* devel : 开发空间
* install : 安装空间
**指令：**
* `catkin_init_workspace` : 创作工作空间（终端在src目录下）
* `catkin_make install` : 建立install空间（终端在工作空间目录下）
* `catkin_make` : 编译工作空间（终端在工作空间目录下）
* `source devel/setup.bash` : 设置环境变量
* `echo $ROS_PACKAGE_PATH` : 检查环境变量

* ` catkin_create_pkg <package_name> [depend1] [depend2] [depend3]`  : 创建功能包（终端在src目录下）

>顺序：新建一个工作空间文件夹 -> 新建一个代码空间文件夹 -> 在代码空间中`catkin_init_workspace` -> 在工作空间中`catkin_make` -> 在工作空间中`catkin_make install`创建install文件夹 -> 在代码空间中` catkin_create_pkg <package_name> [depend1] [depend2] [depend3]`创建功能包 -> 在工作空间中`source devel/setup.bash`设置环境变量 -> 在工作空间中`echo $ROS_PACKAGE_PATH`检查环境变量   

### ROS通信机制
**ROS的核心：分布式通信机制，为用户提供多节点（进程）之间的通信服务**  
ROS三种核心的通信机制：
* 话题通信机制
* 服务通信机制
* 参数管理机制

### 话题通信机制
#### 发布者Publisher
**如何实现一个发布者：**
* 初始化ROS节点 `ros::init()`
* 向ROS Master注册节点信息，包括发布的话题名和话题中的消息类型 `ros::Publisher`
* 创建消息数据
* 按照一定的频率循环发布消息

#### 订阅者Subscriber
**如何实现一个订阅者**
* 初始化ROS节点
* 订阅需要的话题
* 循环等待话题消息，接收到消息后进入回调函数
* 在回调函数中完成消息处理

#### 发布者和订阅者通讯配置
* 创建Publisher.cpp文件,编写c++代码
* 创建Subscriber.cpp文件,编写c++代码
* 修改CMakeLists,在其中加入:
>	add_executable(Publisher_topic_node src/Publisher.cpp)   
>	target_link_libraries(Publisher_topic_node ${catkin_LIBRARIES})
>
>	add_executable(Subscriber_topic_node src/Subscriber.cpp)   
>	target_link_libraries(Subscriber_topic_node ${catkin_LIBRARIES})
* (在工作空间中)运行Publisher和Subscriber:`roscore`
* 新建一个终端，运行Publisher，首先要设置环境变量   
	source /devel/setup.bash   
	rosrun <功能包名> <发布者可执行文件名，Publisher_topic_node>   
* 再新建一个终端，运行Subscriber，也要设置环境变量   
	source /devel/setup.bash   
	rosrun <功能包名> <发布者可执行文件名，Subscriber_topic_node>   

Clion设置环境路径：-DCATKIN_DEVEL_PREFIX:PATH=-DCATKIN_DEVEL_PREFIX:PATH=/home/guo/file/ROS/<工作空间名>/devel

### 服务通信机制

#### 如何实现一个客户端
* 初始化ROS节点
* 创建一个Client实例
* 发布服务请求数据
* 等待Server处理之后的应答结果

#### 如何实现一个服务端
* 初始化ROS节点
* 创建Server实例
* 循环等待服务请求，进入回调函数
* 在回调函数中完成服务功能的处理，并反馈应答数据

`rosservice call/turtle_command "{}"`

#### 服务数据的定义与使用
* 定义srv文件
* 在package.xml中添加功能包依赖
* 在CMakeLists。txt添加编译选项
* 编译生成语言相关文件

### 参数的使用与编程方法
**rosparam**
* `rosparam list`:列出当前有多少参数
* `rosparam get param_key`:显示某个参数值
* `rosparam set param_keyn param_value`:设置某个参数值
* `rosparam dump file_name`:保存参数到文件
* `rosparam load file_name`:从文件读取参数
* `rosparam delete param_key`:删除参数

#### 如何获取/设置参数
* 初始化ROS节点
* get函数获取参数
* set函数设置参数



##命令笔记
* `sudo apt-get install ros-melodic-<功能包名>`: 安装ROS功能包
* `roslaunch <功能包> <launch文件>`: 启动launch文件，launch文件是启动多个节点的文件




	











  
