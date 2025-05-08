# Docker Env
docker run --env="DISPLAY=host.docker.internal:0"  --gpus all --restart unless-stopped --user=ra4ing --privileged=true --volume=C:\Users\ra4ing\Desktop\workspace:/home/ra4ing/workspace --runtime=nvidia --name=base_desktop -itd ra4ing/base:base_ubuntu_24

# docker-env 说明

本目录为多用途开发与安全环境的 Dockerfile 管理目录。

## 目录结构
- base/  基础开发环境（多版本 Ubuntu，含常用开发工具链、shell美化等）
- pwn/   二进制漏洞分析（pwn）环境（多版本 Ubuntu，含pwn工具链、python环境等）
- ros/   机器人操作系统（ROS）相关环境

## Python/conda/pip 说明
- Ubuntu 18 及以下版本统一用conda管理python环境
- Ubuntu 20及以上版本用系统自带python3/pip
- pwn相关python包在Dockerfile中通过pip安装

## 扩展说明
- 各子目录下README.md有详细说明
- 如需添加新工具，请在对应Dockerfile中用apt/pip/conda安装
- shell美化相关请在dotfile/setup.sh中维护

## 用户名自定义说明
- 所有Dockerfile均支持 `--build-arg USER_NAME=yourname` 自定义用户名，默认是 ra4ing。
- dotfile/setup.sh 等脚本均使用 $HOME 变量，支持任意用户名。
