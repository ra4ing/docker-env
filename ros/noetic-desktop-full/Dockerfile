FROM osrf/ros:noetic-desktop-full

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES \
${NVIDIA_VISIBLE_DEVICES:-all}

ENV NVIDIA_DRIVER_CAPABILITIES \
${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

# pre-install softwares
RUN sed -i "s/http:\/\/archive.ubuntu.com/http:\/\/mirrors.tuna.tsinghua.edu.cn/g" /etc/apt/sources.list && \
    sed -i "/security.ubuntu.com/d" /etc/apt/sources.list

RUN apt-get -y update && apt-get upgrade -y
RUN apt-get install -y lib32z1 apt-transport-https
RUN apt-get install -y vim netcat-openbsd openssh-server net-tools
RUN apt-get install -y git unzip curl tmux konsole wget sudo
RUN apt-get install -y python3 python3-pip python3-venv
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
    locales
RUN apt-get install -y \
    libgl1-mesa-dev \
    libglew-dev \
    libsdl2-dev \
    libsdl2-image-dev \
    libglm-dev \
    libfreetype6-dev \
    libglfw3-dev \
    libglfw3 \
    libglu1-mesa-dev \
    freeglut3-dev

# 生成本地化
RUN locale-gen en_US.UTF-8

ARG USER_NAME=ra4ing
ENV USER_NAME=${USER_NAME}
ENV HOME_DIR=/home/${USER_NAME}

RUN useradd -m ${USER_NAME} -s /bin/bash && \
    echo "${USER_NAME}:${USER_NAME}" | chpasswd && \
    usermod -aG sudo ${USER_NAME} && \
    echo "${USER_NAME} ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

USER ${USER_NAME}
WORKDIR /home/${USER_NAME}

ENV PIP_BREAK_SYSTEM_PACKAGES=1
RUN python3 -m pip config set global.index-url http://pypi.tuna.tsinghua.edu.cn/simple && \
    python3 -m pip config set global.trusted-host pypi.tuna.tsinghua.edu.cn && \
    python3 -m pip install --upgrade pip

# enable ssh key login
#RUN mkdir /home/ubuntu/.ssh && \
#    echo "Your ssh key" > /home/ubuntu/.ssh/authorized_keys

RUN git clone https://github.com/ra4ing/dotfile.git /home/${USER_NAME}/.dotfile
RUN cd /home/${USER_NAME}/.dotfile && ./setup.sh

ENV SHELL=/bin/zsh

CMD ["zsh"]

EXPOSE 22
