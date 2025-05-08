FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive
ARG USER_NAME=ra4ing
ENV USER_NAME=${USER_NAME}
ENV HOME_DIR=/home/${USER_NAME}
ENV PYTHON_VERSION=3.11

# pre-install softwares
RUN sed -i "s/http:\/\/archive.ubuntu.com/http:\/\/mirrors.tuna.tsinghua.edu.cn/g" /etc/apt/sources.list && \
    sed -i "/security.ubuntu.com/d" /etc/apt/sources.list

RUN apt-get -y update && apt-get upgrade -y
RUN apt-get install -y lib32z1 apt-transport-https
RUN apt-get install -y vim netcat-openbsd openssh-server net-tools
RUN apt-get install -y git unzip curl tmux konsole wget sudo
RUN apt-get install -y build-essential \
    libncursesw5-dev \
    libssl-dev \
    libpcre3-dev \
    libdb-dev \
    zlib1g-dev \
    libgdbm-dev \
    liblzma-dev \
    uuid-dev \
    libicu-dev \
    libxml2-dev \
    libxslt1-dev \
    libreadline-dev \
    libevent-dev \
    autoconf \
    yodl \
    gettext \
    locales \
    libgmp-dev

# 生成本地化
RUN locale-gen en_US.UTF-8

RUN useradd -m ${USER_NAME} -s /bin/bash && \
    echo "${USER_NAME}:${USER_NAME}" | chpasswd && \
    usermod -aG sudo ${USER_NAME} && \
    echo "${USER_NAME} ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

RUN apt-get install -y software-properties-common && add-apt-repository ppa:deadsnakes/ppa -y
RUN apt-get install -y python${PYTHON_VERSION} python${PYTHON_VERSION}-venv python${PYTHON_VERSION}-distutils python${PYTHON_VERSION}-dev && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python${PYTHON_VERSION} 1 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python${PYTHON_VERSION} 1 && \
    python3 -m ensurepip && \
    update-alternatives --install /usr/bin/pip3 pip3 /usr/local/bin/pip${PYTHON_VERSION} 1 && \
    update-alternatives --install /usr/bin/pip pip /usr/local/bin/pip${PYTHON_VERSION} 1

USER ${USER_NAME}
WORKDIR /home/${USER_NAME}

RUN python3 -m pip config set global.index-url http://pypi.tuna.tsinghua.edu.cn/simple && \
    python3 -m pip config set global.trusted-host pypi.tuna.tsinghua.edu.cn && \
    python3 -m pip install --upgrade pip

# 以下为 pwn 相关 apt 包
RUN sudo dpkg --add-architecture i386 && \
    sudo apt-get update && \
    sudo apt-get install -y libglib2.0-dev libfdt-dev libpixman-1-dev zlib1g-dev \
    libc6-dbg libc6-dbg:i386 bison flex gcc-multilib \
    qemu-system-x86 qemu-user qemu-user-binfmt gcc gdb gdbserver gdb-multiarch clang lldb make cmake \
    bc lsb-release jq libgcc-s1:i386 patchelf

# 安装pwn相关python包（用系统自带python/pip）
ENV PIP_BREAK_SYSTEM_PACKAGES=1
RUN python3 -m pip install --no-cache-dir z3-solver pwntools click ropper LibcSearcher

# enable ssh key login
#RUN mkdir /home/ubuntu/.ssh && \
#    echo "Your ssh key" > /home/ubuntu/.ssh/authorized_keys

RUN git clone https://github.com/ra4ing/dotfile.git /home/${USER_NAME}/.dotfile
RUN cd /home/${USER_NAME}/.dotfile && \
    ./setup.sh          && \
    ./setup_ssh.sh      && \
    ./setup_pwn.sh

ENV SHELL=/bin/zsh

CMD ["zsh"]

EXPOSE 22
