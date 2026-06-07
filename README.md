# ros2-learning
学习记录
# ROS 2 五天练习课程

本仓库用于记录 ROS 2 基础练习过程，目标是在 5 天内熟悉 ROS 2 的核心开发流程，并结合 Git 完成类似真实开发的分支提交、合并和服务器部署流程。

## 环境说明

本项目使用以下环境：

```text
本机开发环境：Windows + CLion / Git
服务器环境：Ubuntu 24.04
ROS 2 版本：Jazzy
安装方式：ros-jazzy-ros-base
构建工具：colcon
开发语言：C++
```

服务器上的 ROS 2 环境需要提前安装：

```bash
sudo apt update
sudo apt install -y ros-jazzy-ros-base ros-dev-tools
sudo apt install -y build-essential cmake git gdb pkg-config
sudo apt install -y python3-colcon-common-extensions
```

每次打开新终端后，需要加载 ROS 2 环境：

```bash
source /opt/ros/jazzy/setup.bash
```

如果已经编译过自己的工作空间，还需要加载工作空间环境：

```bash
source ~/ros2_ws/install/setup.bash
```

---

# 课程目标

完成本课程后，应掌握：

```text
1. ROS 2 workspace 和 package 的基本结构
2. colcon build 的使用方式
3. ROS 2 topic 发布与订阅
4. ROS 2 service 服务端与客户端
5. ROS 2 action 的基本概念
6. launch 文件启动多个节点
7. 自定义 msg / srv / action 接口
8. Git feature 分支开发 + PR 合并流程
9. 在服务器上拉取 main 分支并运行 ROS 2 程序
```

---

# 推荐仓库结构

```text
ros2-learning/
  README.md
  .gitignore

  cpp_pubsub_demo/
    CMakeLists.txt
    package.xml
    src/

  cpp_service_demo/
    CMakeLists.txt
    package.xml
    src/

  cpp_action_demo/
    CMakeLists.txt
    package.xml
    src/

  custom_interfaces/
    CMakeLists.txt
    package.xml
    msg/
    srv/
    action/

  launch_demo/
    CMakeLists.txt
    package.xml
    launch/
    src/
```

服务器上的工作空间结构：

```text
~/ros2_ws/
  src/
    ros2-learning/
  build/
  install/
  log/
```

注意：`build/`、`install/`、`log/` 不提交到 Git。

推荐 `.gitignore`：

```gitignore
build/
install/
log/

cmake-build-*/
.idea/
.vscode/

*.o
*.so
*.a
*.exe

.DS_Store
```

---

# Git 工作流

本项目模拟真实开发流程：

```text
Windows 本机：写代码，提交 feature 分支
远程仓库：创建 Pull Request / Merge Request
main 分支：只保留审查后的稳定代码
Ubuntu 服务器：只拉取 main 分支运行
```

每个功能都从 main 创建新分支：

```bash
git checkout main
git pull origin main
git checkout -b feature/pubsub-demo
```

开发完成后：

```bash
git add .
git commit -m "add pubsub demo"
git push -u origin feature/pubsub-demo
```

然后在 GitHub / Gitee / GitLab 创建合并请求：

```text
feature/pubsub-demo -> main
```

合并后，服务器只拉取 main：

```bash
cd ~/ros2_ws/src/ros2-learning
git checkout main
git pull origin main
```

然后编译运行：

```bash
cd ~/ros2_ws
source /opt/ros/jazzy/setup.bash
colcon build
source install/setup.bash
```

---

# Day 1：ROS 2 环境与基础命令

## 目标

熟悉 ROS 2 的命令行工具、节点、topic、service 的基础概念。

## 练习内容

### 1. 验证 ROS 2 环境

```bash
ros2 --help
ros2 node list
ros2 topic list
ros2 service list
```

### 2. 安装 demo 节点

```bash
sudo apt install -y ros-jazzy-demo-nodes-cpp ros-jazzy-demo-nodes-py
```

### 3. 运行 talker / listener

终端 1：

```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_cpp talker
```

终端 2：

```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_cpp listener
```

如果只有一个 SSH 终端，可以使用后台运行：

```bash
ros2 run demo_nodes_cpp talker &
ros2 run demo_nodes_cpp listener
```

### 4. 观察 topic

```bash
ros2 node list
ros2 topic list
ros2 topic echo /chatter
ros2 topic info /chatter
```

## 今日理解重点

```text
node 是 ROS 2 的运行单元
topic 是发布订阅通信通道
publisher 发布消息
```
