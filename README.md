# color_divide_robot_s_n
ロボット製作論3のS・N班による課題です。

## 実装内容
 
4色(赤、緑、青、黃)のブロックをアームが投げて、色を仕分けることが出来るサンプルコードです。全部で６つのブロックと３つの箱を使い、ブロックは赤色が２つ、緑色が２つ、青色が１つ、黄色が１つ、箱にはそれぞれ違うARマーカーを付けます。ブロックや箱の位置はそれぞれ入れ替えても仕分けが可能です。
 
 ---
## 動作環境

OS: Ubuntu 20.04.5 LTS


ROS1


Gazebo11


Rviz1.14.10


crane_x7_ros


IntelRealSense

---
## 環境構築

1 ROSのインストール

```  
$ git clone https://github.com/ryuichiueda/ros_setup_scripts_Ubuntu18.04_desktop.git
$ cd ros_setup_scripts_Ubuntu18.04_desktop/
$ sudo apt update
$ sudo apt upgrade
$ ./locale.ja.bash
$ ./step0.bash
$ ./step1.bash
```

2 動作確認

```  
$ cd     
$ source ~/.bashrc
$ roscore
```
Ctrl+Cでプログラムの終了

3 ワークスペースを作成し、~/.bashrcを編集

```  
$ cd
$ mkdir -p catkin_ws/src
$ cd ~/catkin_ws/src/
$ catkin_init_workspace
$ cd ..
$ catkin_make
$ vi ~/.bashrc
source /opt/ros/melodic/setup.bash
source ~/catkin_ws/devel/setup.bash       #この行を追加
export ROS_MASTER_URI=http://localhost:11311
$ source ~/.bashrc
$ cd ~/catkin_ws/
$ catkin_make
```

4 CRANE-X7のROSパッケージのインストール

```  
$ cd ~/catkin_ws/src/  
$ git clone https://github.com/rt-net/crane_x7_ros.git
$ git clone https://github.com/roboticsgroup/roboticsgroup_gazebo_plugins.git
$ rosdep install -r -y --from-paths --ignore-src crane_x7_ros
$ ( cd ~/catkin_ws/ && catkin_make )
```  
詳しくは[こちら](https://github.com/rt-net/crane_x7_ros)を参照してください。

5 RVIZの動作確認

```  
$ source ~/.bashrc
$ roscore &
$ rviz
```

6 GAZEBOの動作確認

```  
$ mkdir ~/.ignition/fuel
$ vi config.yaml
config.yamlに以下を追加
servers:
-
  name: osrf
  url: https://api.ignitionrobotics.org
$ roslaunch crane_x7_gazebo crane_x7_with_table.launch
```
  
7 本パッケージのインストール

```  
$ cd ~/catkin_ws/src  
$ git clone  https://github.com/2021-RobotDesign3-team2/crane_x7_ros_test
$ cd ~/catkin_ws
$ catkin_make
```  

8 RealSenseのセットアップ  
[こちら](https://demura.net/robot/16525.html)のサイトを元にRealSense SDKとROSラッパーのインストールをしてください。

9 OpenCVのインストール  
```  
$ wget --no-check-certificate https://raw.githubusercontent.com/milq/milq/master/scripts/bash/install-opencv.sh  
$ chmod +x install-opencv.sh
$ ./install-opencv.sh
$ sudo apt-get install ros-melodic-cv-bridge
```  
## 使用方法

### シミュレーター起動用コマンド

- **~/.bashrc**内の一番下のコードが、以下になるようにしてください。
```bash
source /opt/ros/noetic/setup.bash
source ~/catkin_ws/devel/setup.bash
export ROS_MASTER_URI=http://localhost:11311
export ROS_HOSTNAME=localhost
export GAZEBO_MODEL_PATH=$HOME/catkin_ws/src/color_divide_robot_s_n/models:$GAZEBO_MODEL_PATH
```
- 以上が完了したら、以下のコマンドを使用してビルドしてください。
```bash
$ cd ~/catkin_ws/
$ chmod +x src/color_divide_robot_s_n/scripts/main.py
$ catkin_make
$ source ~/.bashrc
```
- 以上のコードを実行
```
export GAZEBO_MODEL_PATH=$HOME/catkin_ws/src/color_divide_robot_s_n/models:$GAZEBO_MODEL_PATH
```
- gazeboを起動
```
roslaunch color_divide_robot_s_n crane_x7_with_table.launch
```
- main.pyより実行
```
rosrun color_divide_robot_s_n main.py
```
### 実機起動用コマンド


```

