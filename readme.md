navigate to set of goals in yaml forma navgoals python dictionary 



# reinovo_controller使用说明


## 版本

### 2021.3.30-v1.2
修复调度中由于QPlainTextEdit不可在多线程中调用导致卡死

添加部分功能

已相对稳定可用

2021.04.02
由于配套oryx机器人,我们将oryx机器人模型中手臂修改为uarm,以前的模型屏蔽,如果你没有最新的urdf模型,
则将修改后的模型屏蔽以前的打开

### 2020.12.8-v1.1

我修复了部分bug

其他模块123可由参数进行设置

删除导航点可用了


## 依赖

```bash
sudo apt-get install ros-kinetic-qt-create  ros-kinetic-qt-build
```
安装qt5(可选)
## 安装
```bash
cd catkin_ws/src
git clone https://github.com/DylanLN/reinovo_control.git
cd ..
catkin_make
roscd reinovo_control/icon/
sudo sh install.sh
```

有时候会报一些警告,是urdf的错误
```bash
[ WARN] [1589780336.123195128, 85.771000000]: The STL file 'package://aaa/meshes/aaa_01/aaa_01.STL' is malformed. It starts with the word 'solid', indicating that it's an ASCII STL file, but it does not contain the word 'endsolid' so it is either a malformed ASCII STL file or it is actually a binary STL file. Trying to interpret it as a binary STL file instead.
```
需要运行这一句指令
```bash
sed -i 's/^solid/dilos/' ~/oryxbot2_ws/src/oryxbot_description/urdf/pro_links/*
```
其中~/oryxbot2_ws/src/oryxbot_description是功能包所在路径,
需要将此路径修改为你的oryxbot_description功能包的路径

## 打开ui

​    机器人端:

```bash
roslaunch reinovo_control bottom_layer.launch
```

​    我的电脑端（两种方案）:

​        1.点击图标

​        2.使用终端:


```bash
roslaunch reinovo_control reinovo_control.launch
```

## 按钮功能

### 1.首页

![Image text](https://github.com/DylanLN/reinovo_control/blob/alpha/doc/image/home.png)


打开/关闭驱动:	打开/关闭底盘,运动学解算,定向移动 + 激光雷达节点

开始/关闭建图:	打开gmapping地图构建

保存地图:	保存后面输入框 name 的地图到功能包maps文件夹下    (比较大的bug:没有地图话题时不要点击保存,这样会使软件卡死)

open all:	打开/关闭 手臂抓取+自主充电模块

其他模块1:	打开/关闭   手臂抓取

其他模块2:	打开/关闭   自主充电模块

其他模块3:	未设置打开的launch文件,可自行打开


### 2.Teleop

![Image text](https://github.com/DylanLN/reinovo_control/blob/alpha/doc/image/teleop.png)

vx/vy/vth:	控制时的速度

使能:	只有勾选使能才可以控制

teleop topic:	发布速度话题名

↑↓←→:	分别代表前进后退左移右移

左旋/右旋：	左转右转

stop:	点击控制按键时机器人会一直按这个方向移动,点击stop可以让机器人停下来


### 3.导航
![Image text](https://github.com/DylanLN/reinovo_control/blob/alpha/doc/image/nav.png)

地图列表:   reinovo_control功能包maps文件夹下的所有地图,首次使用或地图有增加需要刷新使用

切换地图:   使用map_server节点加载下拉框选择的地图文件

删除地图:   删除下拉框选择的地图

开启导航:   打开导航    (开启导航前需要打开驱动和加载地图(点击切换地图按钮),并且根据位置可能需要重定位)

导航点列表:   你所保存的导航点,需要刷新才能显示和更新

goto:   导航到导航点列表下拉框所框选的地标点

删除:   删除该导航点(因为不能生成文件,所以只能删除程序里的导航点而不能删除文件里的导航点,刷新后全部还有,暂时弃用)(v1.1版实现了此功能)

获取姿态:   获取当前机器人的定位(在导航框架下的定位,必须开启导航),并且将坐标更新到上述 x y th上

保存目标点:   保存ui下的x y th 的数值(并非当前定位,并且xyth保存前可以手动修改),名称在name输入框的导航点


### 4.示教
![Image text](https://github.com/DylanLN/reinovo_control/blob/alpha/doc/image/shijiao.png)

示教列表:   点击刷新可以显示示教任务列表,双击示教列表下的任务可以显示示教信息

示教信息:   显示该示教任务的所有服务,例如抓取放置,ar码追踪,定向移动等,双击该服务可以显示参数信息

参数信息:   参数的数值可以修改

创建示教:   创建示教任务,名称在teach name中

添加:   将action list服务列表下拉框选中选中的服务添加到当前选中的示教任务下,(选中的示教信息服务后面）

删除action:   删除选中的示教信息中的任务(未选中会从删除最上面那个服务)

生成文件:   以上做的所有修改都是在程序里,点击生成文件可以将我们编辑的示教任务保存到文件中(不保存调度无法使用)


### 5.组合
![Image text](https://github.com/DylanLN/reinovo_control/blob/alpha/doc/image/zuhe.png)

path list:   路径列表,需要点击刷新

删除路径:   删除path list路径列表下拉框当前选择的路径

创建路径:   创建一条path name的路径

路径信息:   左边为导航点,右边为示教程序

删除导航点:   删除路径信息中选中的导航点,路径长度以导航点为基准

添加:   添加导航点列表下拉框选中的导航点

挂载:   将视角程序列表下拉框选中的示教程序挂载到当前路径信息中选中的导航点上

生成文件:   将编辑的路径信息保存到文件里,同上,不保存只是编辑在程序里,无法使用


### 6.调度
![Image text](https://github.com/DylanLN/reinovo_control/blob/alpha/doc/image/diaodu.png)

开启调度:   开启调度节点

开始调度:   开始调度

暂停调度:   暂停当前调度

恢复调度:   开发中...

path list:   开启调度前需要刷新路径列表

加载路径:   开发中...      加载当前路径

电压阈值:   开发中...      调度时电压阈值(低于此电压停止工作去充电)

充电时间:   开发中...      每次任务时自主充电时间(单位:分钟)


### 7.机器人状态
![Image text](https://github.com/DylanLN/reinovo_control/blob/alpha/doc/image/zhuangtai.png)

开发中...

显示机器人信息:连接情况/速度/电压/cpu使用率/cpu温度...等等

### 8.使用教程
![Image text](https://github.com/DylanLN/reinovo_control/blob/alpha/doc/image/jiaocheng.png)

 导向官方网站,放二维码开发中

## 功能使用

### 1.底盘控制

首页->打开驱动

Teleop-> 输入xyth速度值

Teleop->点击使能

即可通过下面的前后左右控制

### 2.slam地图构建

首页->打开驱动

首页->开始建图

Teleop->控制建图

### 3.导航

首页->打开驱动

导航->加载地图(选中你所需要的地图)

导航->开启导航

将rviz的Fixed Frame修改为map 并进行定位

### 4.手臂

首页-> 其他模块1

使用服务进行控制抓取

### 6.调度

首页->打开驱动

导航->加载地图(选中你所需要的地图)

导航->开启导航

首页->open all(打开手臂抓取和自主充电)

调度->刷新路径

调度->开启调度(等待一会)

调度->开始调度

## 注意事项

​该软件未使用设计模式,请点击前后顺序不要乱;

由于ros节点开启关闭比较慢所以要耐心等待;

没有地图话题时不要点击保存地图(name为空没事),这样会使软件卡死;

关闭界面前必须关闭驱动和其他按钮!

## 下一步更新

完善基础功能

其他模块按键名称可由非编译参数设置

取消刷新机制

ui按钮增大,适应触摸屏机器人

修改示教重现功能,可以通过ui直接调用服务


## 开源

​    该插件完全开源/解耦,完全可以部署到自己的机器人上(包括示教服务等都可以通过非编译手段进行修改),但是由于软件不稳定,可以暂时修改驱动/地图构建/导航/openall等功能来使用,只需要修改本功能包launch文件夹下相应的launch文件内容(即将他包含的launch文件改为你打开机器人所需要的launch文件)即可使用.

​    我们以驱动为例

```xml
<launch>
    <group ns="robot_bringup">
        <node pkg="reinovo_control" type="blank" name="blank" output="screen" required="true"/>
    </group>
    <!--以上勿动-->
    <include file="$(find oryxbot_base)/launch/oryxbot_base.launch" /> 
    <include file="$(find ydlidar_ros)/launch/X2L.launch" /> 
</launch>
```

​	只需要将我们机器人底盘控制的launch文件oryxbot_base.launch和激光雷达的launch文件X2L.launch修改为你的底盘控制的launch文件即可（注：所有由UI控制的节点的respawn属性不可以打开）.
## 其他
	该软件默认开启分布式通信,主机ip为192,.168.8.100,
    
    如果想关闭打开./icon/reinovo_control.sh文件屏蔽掉:
    
    export ROS_MASTER_URI=http://192.168.8.100:11311
    
    这一句
    
    打开图标可以不显示终端
    
    sudo vi /usr/share/applications/reinovo_control.desktop
    
    将第八行:Terminal=true参数修改为false
## 请联系我
    如果您在使用过程中遇到重大bug or 有更好的想法 or 加入该工程可联系825255961@qq.com  - LN
