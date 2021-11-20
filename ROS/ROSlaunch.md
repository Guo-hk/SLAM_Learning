## ROS 如何书写launch文件

ros日常开发过程中，当我们创建的节点越来越多时，一个一个运行的话效率非常低下，这时我们可以通过输入一条指令来实现所有功能模块的启动。启动文件（Launch File）便是ROS中一种同时启动多个节点的途径，还可以自动启动ROSMaster节点管理器，而且可以实现每个节点的各种配置。

launch文件的使用如下：

```
roslaunch PACKAGE_NAME LAUNCH_FILE_NAME
```

下面介绍如何创建launch文件：

#### 1、标准且简单的launch文件

```
<launch>
   <nodepkg="turtlesim"name="sim1" type="turtlesim_node"/>
   <nodepkg="turtlesim"name="sim2" type="turtlesim_node"/>
</launch>
```

这是一个标准且简单的launch文件，从上面可以看出，launch文件中启动一个节点需要至少三个属性：pkg，name和type。
其中：

```
pkg为节点启动的功能包名称（package名称）
name是用来定义节点运行的名称，它会覆盖节点init()赋予节点的名称
type为节点的可执行文件的名称，执行文件如果是python就填写**.py，若是.cpp就填写编译生成的可执行文件
<node pkg="package-name"type="executable-name" name="node-name" />
```

然而在实际情况中我们会使用到以下属性：

```
·  output = "screen"：将节点的标准输出打印到终端屏幕，默认输出为日志文档；
·  respawn = "true"：复位属性，该节点停止时，会自动重启，默认为false；
·  required = "true"：必要节点，当该节点终止时，launch文件中的其他节点也被终止；
·  ns = "namespace"：命名空间，为节点内的相对名称添加命名空间前缀；
·  args = "arguments"：节点需要的输入参数。
```

#### 2、该标签可以导入另外一个launch文件到当前文件

file ="$(find pkg-name)/path/filename.xml" 指明我们想要包含进来的文件。ns=“NAME_SPACE” 相对NAME_SPACE命名空间导入文件，类似如下：

```
<include file="$(find demo)/launch/demo.launch" ns="demo_namespace"/>
```

#### 3、remap标签顾名思义重映射，

ROS支持topic的重映射，remap标签里包含一个original-name和一个new-name，及原名称和新名称,比如现在你拿到一个节点，这个节点订阅了"/chatter"topic，然而你自己写的节点只能发布到"/demo/chatter"topic，由于这两个topic的消息类型是一致的，你想让这两个节点进行通讯，那么可以在launch文件中这样写:

```
<remap from="chatter" to="demo/chatter"/>
```

这样就可以直接把/chattertopic重映射到/demo/chatter，这样子不用修改任何代码，就可以让两个节点进行通讯。

#### 4、param标签的作用相当于命令行中的rosparam set，用法：

```
<param name="demo_param" type="int" value="666"/>
<param name="output_frame" value="odom"/>
```

#### 5、rosparam标签允许从YAML文件中一次性导入大量参数。 用法：

```
<rosparam command="load" file="$(find pkg-name)/path/name.yaml"/>
```

#### 6、<arg>

argument是另外一个概念，类似于launch文件

内部的局部变量，仅限于launch文件使用，便于launch文件的重构，和ROS节点内部的实现没有关系

```
<paramname="foo" value="$(argarg-name)" />
<node name="node" pkg="package" type="type "args="$(arg arg-name)" />
```

#### 7、实现每个节点在单独的终端显示

（1）节点的定义，只要加入launch-prefix前缀即可：

```
<node pkg="xxx" name="xxx" type="xxx" output="screen" launch-prefix="gnome-terminal -e"> </node>
```

output="screen"为在终端显示
（2）rviz加载配置文件可用以下语句加载：

```
<arg name="command_args" value="-d $(find xxxx)/xxxx.rviz" />
  <node pkg="rviz" type="rviz" name="rviz" args="$(arg command_args)" output="screen"/>
```

