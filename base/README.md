# base 环境 Dockerfiles

本目录包含多版本 Ubuntu 下的基础开发环境 Dockerfile。

## 目录结构
- base_ubuntu_24/  Ubuntu 24.04，基础开发环境
- base_ubuntu_22/  Ubuntu 22.04，基础开发环境
- base_ubuntu_20/  Ubuntu 20.04，基础开发环境
- base_ubuntu_18/  Ubuntu 18.04，基础开发环境

## 主要特性
- 预装常用开发工具链（build-essential, git, curl, tmux等）
- 预装zsh/oh-my-zsh/starship等shell美化（通过setup.sh）

## 扩展说明
- 如需添加新工具，请在对应Dockerfile中安装
- shell美化相关请在dotfile/setup.sh中维护

## 用户名自定义说明
- 构建镜像时可通过 `--build-arg USER_NAME=yourname` 自定义用户名，默认是 ra4ing。
- dotfile/setup.sh 等脚本均使用 $HOME 变量，支持任意用户名。 