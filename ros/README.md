# ROS 环境 Dockerfiles

本目录包含多版本 ROS 桌面环境的 Dockerfile，支持自定义用户名。

## 目录结构
- noetic-desktop-full/  ROS Noetic 桌面环境
- jazzy-desktop-full/   ROS Jazzy 桌面环境

## 用户名自定义说明
- 构建镜像时可通过 `--build-arg USER_NAME=yourname` 自定义用户名，默认是 ra4ing。
- dotfile/setup.sh 等脚本均使用 $HOME 变量，支持任意用户名。

## 配置说明
- ROS 环境在 base 环境基础上，增加了 ROS 相关依赖和配置。
- 其余开发工具链、shell美化等与 base 环境一致。
- ROS 镜像保留了 nvidia 相关环境变量和 ROS 特有依赖。

docker run --env="DISPLAY=host.docker.internal:0" --env="QT_X11_NO_MITSHM=1" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" --env="XAUTHORITY=$XAUTH" --volume="$XAUTH:$XAUTH" --gpus all --restart unless-stopped --user=ra4ing --privileged=true  --volume=workspace_base:/home/ra4ing/workspace --runtime=nvidia --name=ros_jazzy -itd ra4ing/ros:jazzy-desktop-full
