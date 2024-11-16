FROM ubuntu:16.04

ARG DEBIAN_FRONTEND=noninteractive

# pre-install softwares
RUN sed -i "s/http:\/\/archive.ubuntu.com/http:\/\/mirrors.tuna.tsinghua.edu.cn/g" /etc/apt/sources.list && \
    sed -i "/security.ubuntu.com/d" /etc/apt/sources.list

RUN apt-get -y update && apt-get upgrade -y
RUN apt-get -y update && apt-get upgrade -y
RUN apt-get install -y lib32z1 apt-transport-https
RUN apt-get install -y vim netcat-openbsd openssh-server
RUN apt-get install -y git unzip curl tmux konsole wget sudo


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
    bash ./setup.sh && \
    bash ./setup_ssh.sh && \
    bash ./setup_pwn.sh

ENV SHELL=/bin/zsh

CMD ["zsh"]

EXPOSE 22