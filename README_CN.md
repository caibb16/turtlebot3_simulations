# TurtleBot3 仿真

<img src="https://raw.githubusercontent.com/ROBOTIS-GIT/emanual/master/assets/images/platform/turtlebot3/logo_turtlebot3.png" width="300">

## 项目概述

**TurtleBot3 仿真**是一个为ROBOTIS TurtleBot3机器人提供完整仿真环境的ROS 2项目。该项目包含多个仿真包，支持在Gazebo物理引擎中进行机器人模拟测试，以及使用虚拟节点进行快速开发测试。

- **活跃分支**: noetic, humble, main
- **遗留分支**: *-devel
- **版本**: 2.3.8
- **许可证**: Apache 2.0

---

## 项目组成

### 1. **turtlebot3_gazebo** - Gazebo仿真包
一个功能完整的Gazebo仿真平台，用于TurtleBot3机器人的虚拟测试。


**主要目录**:
- `models/` - 机器人URDF模型和场景模型
- `worlds/` - Gazebo仿真环境配置文件
- `launch/` - 仿真启动脚本
- `urdf/` - 机器人通用属性和URDF配置
- `src/` - C++源代码（动态障碍物、交通控制插件等）
- `rviz/` - RViz可视化配置

**依赖**:
- gazebo_ros_pkgs
- geometry_msgs, nav_msgs, sensor_msgs
- rclcpp, tf2
- robot_state_publisher

---

### 2. **turtlebot3_fake_node** - 虚拟节点包
一个轻量级的虚拟机器人节点包，用于无需物理仿真器的快速开发测试。

**主要功能**:
- 无需Gazebo，直接在RViz中进行可视化测试
- 快速的机器人模拟，适合单元测试
- 支持多种机器人型号配置（Burger, Waffle, Waffle Pi）
- 发布机器人状态和传感器信息
---

### 3. **turtlebot3_manipulation_gazebo** - 操作臂仿真包
为TurtleBot3集成OpenMANIPULATOR-X机械臂的Gazebo仿真包。

**主要功能**:
- 支持TurtleBot3+机械臂的综合仿真
- 机械臂控制和夹爪控制
- ROS 2 Control集成
- 支持轨迹规划和抓取仿真

---

### 4. **turtlebot3_simulations** - 元包
主要的元包（Metapackage），用于管理所有仿真子包的依赖。

**依赖**:
- turtlebot3_fake_node
- turtlebot3_gazebo
- turtlebot3_manipulation_gazebo

---

## 快速开始

### 环境要求
- **操作系统**: Ubuntu 20.04 (Noetic) 或 Ubuntu 22.04 (Humble)
- **ROS 2版本**: Noetic或Humble
- **Python**: 3.8+
- **Gazebo**: 11.x或12.x
- **依赖包**: turtlebot3, turtlebot3_msgs

### 安装步骤

#### 1. 创建ROS 2工作空间
```bash
mkdir -p ~/colcon_ws/src
cd ~/colcon_ws
```

#### 2. 克隆项目
```bash
cd src
git clone https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git -b humble
```

#### 3. 安装依赖
```bash
cd ~/colcon_ws
rosdep install --from-paths src --ignore-src -r -y
```

#### 4. 构建项目
```bash
colcon build --symlink-install
```

#### 5. 配置环境
```bash
# 重要：必须先source Gazebo环境变量（设置插件路径等）
source /usr/share/gazebo/setup.sh
# 再source ROS 2环境
source install/setup.bash


# 自动加载方案，无需每次source
# 把这行添加到 ~/.bashrc 的末尾
echo 'source /usr/share/gazebo/setup.sh && source ~/colcon_ws/install/setup.bash' >> ~/.bashrc
# 重新加载配置
source ~/.bashrc
```


### 设置TurtleBot3机器人型号
```bash
export TURTLEBOT3_MODEL=burger    # 或 waffle, waffle_pi
```

---

## 使用示例

### 1. 启动Gazebo仿真环境

#### 启动基础世界
```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

#### 启动空房间
```bash
ros2 launch turtlebot3_gazebo empty_world.launch.py
```

#### 启动TurtleBot3房屋场景
```bash
ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py
```

#### 启动多机器人仿真
```bash
ros2 launch turtlebot3_gazebo multi_robot.launch.py
```

#### 启动DQN学习场景
```bash
ros2 launch turtlebot3_gazebo turtlebot3_dqn_stage1.launch.py
```

#### 启动AutoRace场景
```bash
ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

### 2. 使用虚拟节点（不启动Gazebo）

启动虚拟机器人节点和RViz可视化：
```bash
ros2 launch turtlebot3_fake_node turtlebot3_fake_node.launch.py
```

仅启动虚拟节点：
```bash
ros2 run turtlebot3_fake_node turtlebot3_fake_node
```

### 3. 启动机械臂仿真

基础仿真：
```bash
ros2 launch turtlebot3_manipulation_gazebo gazebo.launch.py
```

虚拟模式：
```bash
ros2 launch turtlebot3_manipulation_gazebo fake.launch.py
```

### 4. 控制机器人

#### 发送导航目标
在另一终端中运行：
```bash
ros2 run turtlebot3_gazebo turtlebot3_drive
```

或使用RViz中的Navigation工具进行可视化导航。

#### 发布速度指令
```bash
ros2 topic pub /cmd_vel geometry_msgs/Twist '{linear: {x: 0.1, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.1}}'
```

---

## 项目结构详解

### Gazebo仿真场景

| 场景 | 用途 | 描述 |
|------|------|------|
| `turtlebot3_world.launch.py` | 基础测试 | 带障碍物的标准测试环境 |
| `empty_world.launch.py` | 简单测试 | 空房间，用于基础开发 |
| `turtlebot3_house.launch.py` | 导航测试 | 室内房屋环境 |
| `multi_robot.launch.py` | 多机器人 | 支持多个机器人同时仿真 |
| `turtlebot3_autorace_2020.launch.py` | 自动竞赛 | 自动赛车竞赛环境 |
| `turtlebot3_dqn_stage*.launch.py` | 强化学习 | DQN学习的多阶段环境 |

### 机器人型号

| 型号 | 特点 | 用途 |
|------|------|------|
| **Burger** | 小型、低成本、基础功能 | 教学、入门、轻量级应用 |
| **Waffle** | 中等规模、增强功能 | 研究、复杂应用 |
| **Waffle Pi** | 集成Raspberry Pi、配备摄像头 | 视觉导航、高级应用 |

### 主要ROS 2话题和服务

#### 发布话题
- `/odom` - 里程计信息
- `/scan` - LiDAR扫描数据
- `/camera/image_raw` - 摄像头图像（Waffle Pi）
- `/cmd_vel_out` - 实际执行的速度（来自控制器）
- `/tf` - 变换框架

#### 订阅话题
- `/cmd_vel` - 速度指令输入

#### 服务
- `/reset_simulation` - 重置仿真
- `/pause_physics` - 暂停物理引擎
- `/unpause_physics` - 恢复物理引擎

---

## 常见应用场景

### 1. 路径规划与导航
使用Gazebo仿真环境测试路径规划算法（A*、Dijkstra等）。

```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
# 在另一个终端中启动导航算法
```

### 2. SLAM（同步定位与建图）
在虚拟世界中测试SLAM算法。

```bash
# 启动仿真 + SLAM节点
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

### 3. 强化学习训练
使用DQN环境进行机器学习训练。

```bash
ros2 launch turtlebot3_gazebo turtlebot3_dqn_stage1.launch.py
```

### 4. 多机器人协作
仿真多个机器人的协调控制。

```bash
ros2 launch turtlebot3_gazebo multi_robot.launch.py
```

### 5. 快速单元测试
使用虚拟节点进行CI/CD测试。

```bash
ros2 launch turtlebot3_fake_node turtlebot3_fake_node.launch.py
```

---


## 调试与故障排除

### 0. spawn_entity服务不可用
**问题**: 
```
[spawn_entity.py-4] [ERROR]: Service /spawn_entity unavailable. 
Was Gazebo started with GazeboRosFactory?
```

**原因**:
- 未source `/usr/share/gazebo/setup.sh`，导致插件路径不正确
- 或gzserver进程异常退出，gazebo_ros_factory插件未初始化

**解决方案**:
```bash
# 确保按顺序执行
source /usr/share/gazebo/setup.sh      # 必须先执行
source ~/colcon_ws/install/setup.bash
export TURTLEBOT3_MODEL=burger

# 验证环境变量已设置
echo $GAZEBO_PLUGIN_PATH
echo $GAZEBO_MODEL_PATH

# 重新启动
ros2 launch turtlebot3_gazebo empty_world.launch.py
```

