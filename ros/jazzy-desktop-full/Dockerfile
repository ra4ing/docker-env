FROM osrf/ros:jazzy-desktop-full

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES \
${NVIDIA_VISIBLE_DEVICES:-all}

ENV NVIDIA_DRIVER_CAPABILITIES \
${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

# pre-install softwares
RUN apt-get update --fix-missing || true
RUN apt-get install -y --no-install-recommends ca-certificates
RUN cp /etc/apt/sources.list.d/ubuntu.sources /etc/apt/sources.list.d/ubuntu.sources.bak
RUN echo "Types: deb\n\
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu/\n\
Suites: noble noble-updates noble-backports noble-security\n\
Components: main restricted universe multiverse\n\
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg" > /etc/apt/sources.list.d/ubuntu.sources

RUN apt-get -y update && apt-get upgrade -y
RUN apt-get install -y lib32z1 apt-transport-https
RUN apt-get install -y vim netcat-openbsd openssh-server
RUN apt-get install -y git unzip curl tmux konsole wget sudo

RUN apt-get update && \
apt-get install -y \
build-essential \
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


RUN useradd -m ra4ing -s /bin/bash && \
    echo "ra4ing:ra4ing" | chpasswd && \
    usermod -aG sudo ra4ing && \
    echo 'ra4ing ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers


USER ra4ing
WORKDIR /home/ra4ing

# enable ssh key login
#RUN mkdir /home/ubuntu/.ssh && \
#    echo "Your ssh key" > /home/ubuntu/.ssh/authorized_keys

RUN git clone https://github.com/ra4ing/dotfile.git /home/ra4ing/.dotfile
RUN cd /home/ra4ing/.dotfile && \
    ./setup.sh          && \
    ./setup_ssh.sh      && \
    ./setup_pwn.sh

ENV SHELL=/bin/zsh

CMD ["zsh"]

EXPOSE 22
