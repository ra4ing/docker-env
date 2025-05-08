# docker file for pwn env

# pwn 环境 Dockerfiles

本目录包含多版本 Ubuntu 下的二进制漏洞分析（pwn）环境 Dockerfile。

## 目录结构
- pwn_ubuntu_24/  Ubuntu 24.04，基础pwn环境
- pwn_ubuntu_22/  Ubuntu 22.04，基础pwn环境
- pwn_ubuntu_20/  Ubuntu 20.04，基础pwn环境
- pwn_ubuntu_18/  Ubuntu 18.04，基础pwn环境
- pwn_ubuntu_16/  Ubuntu 16.04，基础pwn环境

## 主要特性
- 预装常用pwn工具链（gdb, pwntools, qemu, gdb-multiarch等）
- 预装zsh/oh-my-zsh/starship等shell美化（通过setup.sh）

## 扩展说明
- 如需添加新工具，请在对应Dockerfile中用apt/pip/conda安装
- shell美化相关请在dotfile/setup.sh中维护
- pwn相关python包在Dockerfile中通过pip安装

## 用户名自定义说明
- 构建镜像时可通过 `--build-arg USER_NAME=yourname` 自定义用户名，默认是 ra4ing。
- dotfile/setup.sh 等脚本均使用 $HOME 变量，支持任意用户名。
