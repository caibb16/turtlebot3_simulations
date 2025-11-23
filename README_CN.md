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

**主要功能**:
- 支持多种机器人模型（Burger, Waffle, Waffle Pi）
- 包含多种预设仿真场景（空房间、房屋、AutoRace、DQN学习环境等）
- 支持多机器人同时仿真
- 集成ROS 2控制和传感器模拟
- 包含动态插件（交通灯、障碍物等）

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

**用途**:
- 快速开发调试
- 单元测试
- CI/CD流程中的轻量级测试
- 学习ROS 2机器人编程

**主要文件**:
- `src/turtlebot3_fake_node.cpp` - 虚拟节点实现
- `param/` - 机器人参数配置（YAML格式）
- `launch/` - 启动脚本
- `include/` - 头文件

**依赖**:
- rclcpp, geometry_msgs, nav_msgs, sensor_msgs
- tf2, tf2_msgs
- turtlebot3_msgs
- robot_state_publisher

---

### 3. **turtlebot3_manipulation_gazebo** - 操作臂仿真包
为TurtleBot3集成OpenMANIPULATOR-X机械臂的Gazebo仿真包。

**主要功能**:
- 支持TurtleBot3+机械臂的综合仿真
- 机械臂控制和夹爪控制
- ROS 2 Control集成
- 支持轨迹规划和抓取仿真

**主要目录**:
- `urdf/` - 机械臂和完整系统的URDF模型
- `gazebo/` - Gazebo特定配置
- `meshes/` - 3D模型文件
- `ros2_control/` - ROS 2控制配置
- `config/` - 控制器配置文件
- `launch/` - 仿真启动脚本

**依赖**:
- gazebo_ros
- ros2_control, ros2_controllers
- gripper_controllers
- robot_state_publisher, xacro

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
source install/setup.bash
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

## 配置文件详解

### 机器人参数文件（param/）

每个型号都有对应的YAML配置文件：

**burger.yaml** - Burger机器人参数
**waffle.yaml** - Waffle机器人参数
**waffle_pi.yaml** - Waffle Pi机器人参数

参数包括：
- 物理参数（质量、轮子半径等）
- 传感器配置（LiDAR参数、摄像头）
- 控制器参数（速度限制、加速度等）

### Gazebo世界文件（worlds/）

这些`.world`文件定义了仿真环境，包括：
- 地面和墙壁
- 静态障碍物
- 灯光和物理引擎设置
- 预设模型位置

### URDF模型文件（urdf/）

URDF（统一机器人描述格式）文件定义了：
- 机器人的物理结构（链接和关节）
- 传感器安装点
- 视觉属性（颜色、纹理）
- 碰撞几何体

---

## 调试与故障排除

### 1. 模型加载失败
**问题**: Gazebo无法加载模型
**解决方案**:
```bash
# 确保设置正确的环境变量
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:$(pwd)/turtlebot3_gazebo/models
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

### 2. 话题未发布
**问题**: 无法接收到机器人传感器数据
**解决方案**:
```bash
# 检查话题是否存在
ros2 topic list
# 查看话题内容
ros2 topic echo /scan
# 查看节点状态
ros2 node list
```

### 3. 机器人不动
**问题**: 发送速度指令后机器人无反应
**解决方案**:
```bash
# 检查速度指令是否被接收
ros2 topic echo /cmd_vel
# 确认Gazebo物理引擎未暂停
ros2 service call /unpause_physics std_srvs/Empty
```

### 4. 性能问题
**问题**: 仿真运行缓慢
**解决方案**:
- 减少物理引擎的迭代次数
- 关闭不必要的传感器
- 使用`empty_world.launch.py`进行测试
- 检查系统资源使用情况

---

## 相关开源项目

本项目是TurtleBot3生态系统的一部分，相关项目包括：

- [turtlebot3](https://github.com/ROBOTIS-GIT/turtlebot3) - 主机器人包
- [turtlebot3_msgs](https://github.com/ROBOTIS-GIT/turtlebot3_msgs) - 消息定义
- [turtlebot3_manipulation](https://github.com/ROBOTIS-GIT/turtlebot3_manipulation) - 机械臂主包
- [turtlebot3_manipulation_simulations](https://github.com/ROBOTIS-GIT/turtlebot3_manipulation_simulations) - 机械臂仿真
- [turtlebot3_applications](https://github.com/ROBOTIS-GIT/turtlebot3_applications) - 应用示例
- [turtlebot3_machine_learning](https://github.com/ROBOTIS-GIT/turtlebot3_machine_learning) - 机器学习示例
- [turtlebot3_autorace](https://github.com/ROBOTIS-GIT/turtlebot3_autorace) - 自动竞赛包
- [turtlebot3_home_service_challenge](https://github.com/ROBOTIS-GIT/turtlebot3_home_service_challenge) - 家务挑战
- [open_manipulator](https://github.com/ROBOTIS-GIT/open_manipulator) - OpenMANIPULATOR机械臂
- [dynamixel_sdk](https://github.com/ROBOTIS-GIT/DynamixelSDK) - Dynamixel舵机SDK
- [hls_lfcd_lds_driver](https://github.com/ROBOTIS-GIT/hls_lfcd_lds_driver) - LiDAR驱动
- [ld08_driver](https://github.com/ROBOTIS-GIT/ld08_driver) - LiDAR驱动

---

## 文档和学习资源

### 官方文档
- ⚙️ **[ROBOTIS DYNAMIXEL](https://dynamixel.com/)** - Dynamixel舵机官方网站
- 📚 **[Dynamixel SDK使用指南](http://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_sdk/overview/)** - SDK官方文档
- 📚 **[TurtleBot3官方使用手册](http://turtlebot3.robotis.com/)** - 完整的TurtleBot3指南
- 📚 **[OpenMANIPULATOR-X使用手册](https://emanual.robotis.com/docs/en/platform/openmanipulator_x/overview/)** - 机械臂指南

### 视频教程
- 🎥 **[ROBOTIS官方YouTube频道](https://www.youtube.com/@ROBOTISCHANNEL)** - 官方视频资源
- 🎥 **[ROBOTIS开源团队频道](https://www.youtube.com/@ROBOTISOpenSourceTeam)** - 开源项目视频
- 🎥 **[TurtleBot3视频播放列表](https://www.youtube.com/playlist?list=PLRG6WP3c31_XI3wlvHlx2Mp8BYqgqDURU)** - TurtleBot3专题视频
- 🎥 **[OpenMANIPULATOR视频播放列表](https://www.youtube.com/playlist?list=PLRG6WP3c31_WpEsB6_Rdt3KhiopXQlUkb)** - 机械臂专题视频

### 社区支持
- 💬 **[ROBOTIS社区论坛](https://forum.robotis.com/)** - 获取帮助和讨论

---

## 许可证

本项目采用 **Apache 2.0** 许可证。详见[LICENSE](LICENSE)文件。

### 贡献要求

任何贡献都将基于Apache 2许可证。贡献者必须在每个提交中添加`Signed-off-by: ...`行，以证明他们有权提交代码。详见[CONTRIBUTING.md](CONTRIBUTING.md)。

---

## 开发者信息

**维护者**: Pyo (pyo@robotis.com)

**主要贡献者**:
- Darby Lim (thlim@robotis.com)
- Pyo (pyo@robotis.com)
- Ryan Shim
- Will Son (willson@robotis.com)
- Hye-jong KIM (hjkim@robotis.com)
- Hyungyu Kim (kimhg@robotis.com)

---

## 常见问题（FAQ）

**Q: 我应该使用哪个分支？**
A: 如果使用ROS 2 Humble，选择`humble`分支；如果使用ROS 2 Noetic，选择`noetic`分支。`main`分支是最新开发版本。

**Q: Gazebo仿真和虚拟节点有什么区别？**
A: Gazebo提供完整的物理引擎仿真，适合复杂场景和研究；虚拟节点是轻量级模拟，适合快速开发和测试。

**Q: 如何在CI/CD中使用此项目？**
A: 使用`turtlebot3_fake_node`进行单元测试，因为它不需要Gazebo和图形界面。

**Q: 支持哪些ROS 2版本？**
A: 支持Noetic、Humble和最新的main分支。

**Q: 如何贡献代码？**
A: Fork本仓库，创建功能分支，提交PR。所有贡献必须遵守Apache 2.0许可证并进行DCO签名。

---

**最后更新**: 2024年
**项目版本**: 2.3.8
