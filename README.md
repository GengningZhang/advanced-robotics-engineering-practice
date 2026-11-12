
# 机器人工程高级实践

## 课程信息

| 项目 | 信息 |
| --- | --- |
| **主讲教师**| 周易|
| **助教（TA）** | 张耿宁（`xxx@xxx`）、王浩（`xxx@xxx`） |
| **实验室主页** | [NAIL Lab](https://sites.google.com/view/zhouyi-joey/home) |
| **开课时间** | 2027 年春季学期 |


本课程围绕**自主无人机的感知、导航与控制**展开。课程从四旋翼运动控制出发，逐步学习轨迹生成、路径规划、视觉定位与多传感器融合，并通过编程实验将这些模块连接起来。

本仓库整理了课程的实验代码及相关说明，主要面向希望动手理解机器人算法、完成仿真与 ROS 实践的同学。

## 课程内容

完成课程中的理论学习和实验后，你将接触到一套自主无人机系统的主要算法模块：

- **建模与控制**：理解四旋翼刚体运动、动力学模型及轨迹跟踪控制。
- **轨迹生成与路径规划**：利用多项式优化生成平滑轨迹，并通过 A* 搜索规划避障路径。
- **视觉感知与定位**：学习相机投影模型、PnP 位姿估计、特征跟踪和双目视觉里程计。
- **状态估计与传感器融合**：利用 EKF 融合 IMU 与视觉测量，估计无人机的位置、姿态和速度。
- **工程实现**：使用 Python、C++ 和 ROS 组织算法，完成仿真、数据回放与可视化。

## 实验项目

| 项目 | 实验内容 | 
| --- | --- |
| **Project 2：四旋翼轨迹跟踪** | 实现四旋翼控制器，完成悬停及给定轨迹的跟踪仿真。|
| **Project 3：轨迹生成** | 通过多项式优化生成平滑轨迹，理解 Minimum Jerk / Minimum Snap。 |
| **Project 4：路径规划** | 使用 A* 搜索避障路径，并结合轨迹生成与控制完成仿真。|
| **Project 5：3D-2D 位姿估计** | 基于 Marker 角点实现平面 DLT PnP，恢复相机的旋转和平移。 |
| **Project 6：双目视觉里程计** | 完成特征提取、光流跟踪、双目三角化和相机运动估计。 | 
| **Project 7：基于 EKF 的 IMU 与视觉融合** | 使用 IMU 进行状态预测，利用 Marker PnP 进行观测更新，输出融合位姿及轨迹。 |

建议按项目顺序学习：后续实验会用到前面完成的控制、轨迹生成或视觉定位模块。

## 实验环境

实验主要使用 **Python 3、C++ 和 ROS 1**。Project 2–4 以 Python 仿真为主；Project 5–7 使用 ROS、OpenCV、Eigen 和 RViz，并通过 rosbag 回放传感器数据。

对于 ROS 实验，推荐 **Ubuntu 20.04 + ROS Noetic**。如果不想单独配置 ROS 依赖，也可以使用 [HKUST 2026 课程提供的 Docker 环境](https://github.com/HKUST-Aerial-Robotics/HKUST-ELEC5660-Introduction-to-Aerial-Robotics/blob/2026/assignments/README.md)。

### 1. Python 仿真（Project 2–4）

进入相应项目目录，安装其 `requirements.txt` 中列出的依赖后运行：

```bash
python3 -m pip install -r requirements.txt
python3 app.py
```

按照终端提示在浏览器中打开仿真页面。不同项目的控制器、轨迹生成器和规划模块以各自实验说明为准。

### 2. ROS 工程（Project 5–7）

先配置好 ROS 1 工作空间，并准备 OpenCV、Eigen3、ArUco 等依赖。进入**对应项目的 `catkin_ws` 目录**后执行：

```bash
catkin_make
source devel/setup.bash
```

随后启动相应的实验：

```bash
# Project 5
roslaunch tag_detector bag_tag.launch

# Project 6
roslaunch stereo_vo stereo_vo_bag.launch

# Project 7
roslaunch ekf A3.launch
```

三个项目分别使用各自的 ROS 工作空间。运行前请确认数据包、配置文件和需要复用的上一项目代码已经准备好。

## ROS 数据包下载（Project 5–7）

后面三个实验的数据包体积较大，不随源码仓库一起上传。**以下命令在上游仓库根目录执行**；下载时请将目标路径换成实际工程中的 `bag/` 目录。

**Project 5 — Marker PnP（`images.bag`）**

```bash
wget -P assignments/proj2phase1/catkin_ws/src/tag_detector/bag \
  https://repo.hkust-uav.org/artifactory/generic-artifactory/rosbag/elec5660/images.bag
```

**Project 6 — 双目视觉里程计（`realsense_1.bag`）**

```bash
wget -P assignments/proj2phase2/catkin_ws/src/stereo_vo/stereo_vo_estimator/bag \
  https://repo.hkust-uav.org/artifactory/generic-artifactory/rosbag/elec5660/realsense_1.bag
```

**Project 7 — EKF 融合（`ekf_A3.bag`）**

```bash
wget -P assignments/proj3phase1/catkin_ws/src/ekf/bag \
  https://repo.hkust-uav.org/artifactory/generic-artifactory/rosbag/elec5660/ekf_A3.bag
```

下载完成后，将数据包放在对应 ROS 启动文件所要求的位置，再编译运行。

## 参考与致谢

本课程的部分实验内容与代码框架参考了香港科技大学的 [ELEC5660: Introduction to Aerial Robotics](https://github.com/HKUST-Aerial-Robotics/HKUST-ELEC5660-Introduction-to-Aerial-Robotics/tree/2026)，并根据《机器人工程高级实践》的教学安排进行了整理与调整。感谢原课程团队公开相关教学资源。

使用本仓库时，请以各项目的实验说明和实际代码为准。如有环境配置或实验内容方面的问题，可联系课程助教（邮箱见上方课程信息）。
