## 原仓库：free_fleet
https://github.com/open-rmf/free_fleet.git

本代码适用于快速部署客户端的情况，首先需要准备带有docker环境的电脑
docker环境：ubuntu 20.04 ros noetic
## docker镜像安装
```bash
docker push lagrangeluo/limo_fleet_server:V1
```
然后准备工作空间，本项目才用了docker环境和开发空间分离解藕的方式，docker容器里只包含了必须的环境，具体的代码需要自己clone本仓库的源码

执行以下命令建立工作空间：
```bash
mkdir YOUR_WS_NAME
cd YOUR_WS_NAME
mkdir src
cd src
git clone https://github.com/lagrangeluo/free_fleet_client_ros1.git
```
注意：YOUR_WS_NAME需要改成用户自己的任意工作空间名字
### 创建容器
注意，创建容器之前需要在宿主机上建好工作空间文件夹，路径如下命令行所示：/home/agilex/YOUR_WS_NAME，这里agilex是我的主机用户名，用户可以自行设置，只要保证下面对应部分都有所修改。
```bash
sudo docker run -it --network host --privileged --name limo_server --device-cgroup-rule="a *:* rmw" -v /dev:/dev -v /home/agilex/YOUR_WS_NAME:/home/YOUR_WS_NAME lagrangeluo/limo_fleet_server:V1
```
执行以上命令后当前窗口就进入了刚刚创建的docker容器中了，使用vscode来管理container中的文件系统，可以发现home下的工作空间没有代码。

## 下载cyclonedds
```bash
git clone https://github.com/eclipse-cyclonedds/cyclonedds -b releases/0.7.x
```
## 检查相关依赖是否安装
```bash
cd ~/YOUR_WS_NAME
rosdep install --from-paths src --ignore-src --rosdistro noetic -yr
```
## 编译
```bash
cd ~/YOUR_WS_NAME
source /opt/ros/noetic/setup.bash
colcon build
```
根据自己的需求，修改客户端的参数，包括车队名和机器人名
## 运行client
```bash
roslaunch ff_example_ros1 limo_ff.launch
```