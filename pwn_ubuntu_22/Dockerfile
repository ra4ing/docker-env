FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive

# pre-install softwares
RUN sed -i "s/http:\/\/archive.ubuntu.com/http:\/\/mirrors.tuna.tsinghua.edu.cn/g" /etc/apt/sources.list && \
    sed -i "/security.ubuntu.com/d" /etc/apt/sources.list
RUN dpkg --add-architecture i386
RUN apt-get -y update && apt-get upgrade -y
RUN apt-get install -y lib32z1 apt-transport-https
RUN apt-get install -y python3 python3-pip python3-venv python3-dev
RUN apt-get install -y libglib2.0-dev libfdt-dev libpixman-1-dev zlib1g-dev libc6-dbg libc6-dbg:i386 libgcc-s1:i386
RUN apt-get install -y vim nano netcat-openbsd openssh-server git unzip curl tmux konsole wget sudo
RUN apt-get install -y bison flex build-essential gcc-multilib
RUN apt-get install -y qemu-system-x86 qemu-user qemu-user-binfmt
RUN apt-get install -y gcc gdb gdbserver gdb-multiarch clang lldb make cmake

# enable ssh login
RUN rm -f /etc/service/sshd/down
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config &&\
    sed -ri 's/#UseDNS\ no/UseDNS\ no/g' /etc/ssh/sshd_config && \
    sed -ri "s/StrictModes yes/StrictModes no/g" /etc/ssh/sshd_config && \
    sed -ri "s/UsePAM yes/UsePAM no/g" /etc/ssh/sshd_config

RUN mkdir /run/user/1000/fnm_multishells -p && chmod a+w /run/user/1000/fnm_multishells

# enable login with password
RUN echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config

# Create flag files with contents
RUN echo "flag{this_is_a_flag}" > /flag && \
    echo "flag{this_is_a_flag}" > /flag.txt && \
    chmod 400 /flag /flag.txt

# defaultly we have a user `ubuntu` in the image
RUN useradd -m ra4ing -s /bin/bash && \
    echo "ra4ing:ra4ing" | chpasswd && \
    usermod -aG sudo ra4ing && \
    echo 'ra4ing ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers


USER ra4ing
WORKDIR /home/ra4ing

# enable ssh key login
#RUN mkdir /home/ubuntu/.ssh && \
#    echo "Your ssh key" > /home/ubuntu/.ssh/authorized_keys

# create venv for pip
RUN python3 -m venv /home/ra4ing/.pip_venv

# pwn-related tools
RUN . /home/ra4ing/.pip_venv/bin/activate && \
    pip install --upgrade pip && \
    pip config set global.index-url http://pypi.tuna.tsinghua.edu.cn/simple && \
    pip config set global.trusted-host pypi.tuna.tsinghua.edu.cn && \
    pip install --no-cache-dir \
    pwntools \
    pwncli \
    ropgadget \
    z3-solver \
    smmap2 \
    apscheduler \
    ropper \
    unicorn \
    keystone-engine \
    capstone \
    angr \
    pebble \
    r2pipe \
    LibcSearcher \
    poetry \
    pipx

RUN git clone https://github.com/ra4ing/dotfile.git /home/ra4ing/.dotfile
RUN cd /home/ra4ing/.dotfile && \
    bash ./setup.sh

ENV SHELL=/bin/zsh

CMD ["/bin/zsh"]

EXPOSE 22
