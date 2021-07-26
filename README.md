# Dronekit-configure-in-unbuntu
Unbuntu-SITL-MAVProxy

学习记录

# 一、名词概念 #

## 1. Dronekit-Python  ##
	- 一个用于控制无人机的Python 库，提供了用于无人机的API，其代码独立于飞控，单独运行在机载电脑或其他设备上，通过串口或无线的方式经 *MAVLink* 协议与飞控板通信。
## 2. MAVLink  ##
	- 是在串口通讯基础上的一种更高层的开源通讯协议，主要应用在微型飞行器的通讯上。为小型飞行器和地面站通讯时常用数据制定了一种发送和接受的规则，并加入了校验功能。协议以消息库的形式定义了参数传输的规则。
	- 是一个为微型飞行器设计的非常轻巧的、只由头文件构成的信息编组库。它可以通过串口非常高效地封装C结构数据，并将这些数据包发送至地面控制站。该协议被PX4, PIXHAWK, APM和Parrot AR.Drone平台所广泛测试并在以上的项目中作为MCU/IMU间以及Linux进程和地面站链路通信间的主干通信协议。

## 3. SITL ##
	- 开源软件模拟器
	- 用户可以在不依托任何硬件的情况下，对固定翼(Plane)、旋翼机(Copter)和车辆(Rover)进行模拟。除了进行路径规划测试、参数设置测试、控制代码调试以外，SITL还可模拟风力、地形等外部环境，安装模拟光流、激光雷达等传感器，与其他设备通过串口、网络通信等等。
## 4. MAVProxy ##
	- 数据转发软件,基于 MAVLink 的系统的无人机地面站软件包
	- MAVProxy 是一款用于无人机的全功能 GCS，设计为一款简约、便携和可扩展的 GCS，适用于任何支持 MAVLink 协议的自主系统（例如使用 ArduPilot 的系统）。MAVProxy 是一个功能强大的基于命令行的“开发者”地面站软件。它可以通过附加模块进行扩展，或与另一个地面站（例如 Mission Planner、APM Planner 2、QGroundControl 等）相辅相成，以提供图形用户界面。

# 二、配置步骤   #
## 1.在unbuntu中安装python2(dronekit包不支持python3)  
bash中：  

    sudo apt install python

## 2.安装dronekit、dronekit-sitl(本地仿真工具，见名词解释3)、mavproxy 
bash中：

	pip install dronekit
	pip install dronekit-sitl
	pip install mavproxy
注：此步骤可能出现的错误 pip命令无法识别，则，安装pip命令  
bash中：

	sudo apt install python-pip


## 3.启动SITL，并配置home点、机型 
bash中：

	dronekit-sitl  copter --home=39.9,116.38,3,30 --model=quad

--home=纬度，经度，3,30 经纬度可自行设置  
## 4.启动mavproxy数据转发服务 
另开一个bash窗口，cd到mavproxy.py文件目录：

	cd ~/.local/bin/
	python mavproxy.py  --master=tcp:127.0.0.1:5760  --out=***missionPlanner电脑的IP:自拟端口号***  --out=127.0.0.1:14550 

tcp:127.0.0.1:5760： SITL默认端口，作为mavproxy的输入，把输入数据转发到如下两个端口  
127.0.0.1:14550 ： 本机地址14550端口，供本地python程序连接(此端口号对应py程序中连接端口号)   
*missionPlanner电脑的IP:自拟端口号*：同一局域网内另一台电脑地址(若unbuntu运行在虚拟机中，此处可在Windows本机IP)，供MissionPlanner连接  
## 5.启动MissionPlanner 
右上角选择UDP连接，端口号填写步骤4中自拟端口号  
注：此步骤可能出现地图无法加载，则  
左上角***飞行计划***->右侧地图选项选择高德(或其他)
## 6.运行dronekit程序 .py文件  
再另新开一个bash：  
	cd .py文件目录
	python 文件名.py  
此步将在MissionPlanner中看到你得dronekit程序控制的无人机运行状况。


## 草稿，待整理 ##

安装需要的依赖  
	
	sudo apt-get install python-dev python3-dev libxml2-dev libxslt1-dev zlib1g-dev
	git clone https://github.com/dronekit/dronekit-python.git
	cd ./dronekit-python
	sudo python setup.py build
	sudo python setup.py install
