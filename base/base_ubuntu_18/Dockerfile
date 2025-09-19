FROM ubuntu:18.04

ARG DEBIAN_FRONTEND=noninteractive
ARG USER_NAME=ra4ing
ENV USER_NAME=${USER_NAME}
ENV HOME_DIR=/home/${USER_NAME}
ENV PYTHON_VERSION=3.11.12

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
    libncurses5-dev \
    libbz2-dev \
    libsqlite3-dev \
    libffi-dev

# 生成本地化
RUN locale-gen en_US.UTF-8

RUN if id ubuntu >/dev/null 2>&1; then \
        userdel ubuntu; \
        rm -rf /home/ubuntu; \
    fi
RUN useradd -m -u 1000 -s /bin/bash ${USER_NAME} && \
    echo "${USER_NAME}:${USER_NAME}" | chpasswd && \
    usermod -aG sudo ${USER_NAME} && \
    echo "${USER_NAME} ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

RUN apt-get remove -y openssl && \
    wget https://www.openssl.org/source/openssl-1.1.1g.tar.gz --no-check-certificate && \
    tar -xzf openssl-1.1.1g.tar.gz && \
    cd openssl-1.1.1g && \
    ./config && \
    make -j$(nproc) && \
    make install && \
    cp -r libssl.so.1.1 /usr/lib && \
    cp -r libcrypto.so.1.1 /usr/lib && \
    cd .. && \
    rm -rf openssl-1.1.1g openssl-1.1.1g.tar.gz
RUN wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz --no-check-certificate && \
    tar -xzf Python-${PYTHON_VERSION}.tgz && \
    cd Python-${PYTHON_VERSION} && \
    ./configure --enable-optimizations \
        --with-openssl=/usr/local \
        --with-openssl-rpath="/usr/local/lib" \
        --with-ensurepip=install \
        CFLAGS="-I/usr/include" \
        LDFLAGS="-L/usr/lib" && \
    make -j$(nproc) && \
    make altinstall && \
    cd .. && \
    rm -rf Python-${PYTHON_VERSION} Python-${PYTHON_VERSION}.tgz

ENV PIP_BREAK_SYSTEM_PACKAGES=1
RUN update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.11 1 && \
    update-alternatives --install /usr/bin/python python /usr/local/bin/python3.11 1 && \
    python3 -m ensurepip && \
    update-alternatives --install /usr/bin/pip3 pip3 /usr/local/bin/pip3.11 1 && \
    update-alternatives --install /usr/bin/pip pip /usr/local/bin/pip3.11 1

USER ${USER_NAME}
WORKDIR /home/${USER_NAME}

RUN python3 -m pip config set global.index-url http://pypi.tuna.tsinghua.edu.cn/simple && \
    python3 -m pip config set global.trusted-host pypi.tuna.tsinghua.edu.cn && \
    python3 -m pip install --upgrade pip && \
    python3 -m pip install --no-cache-dir certifi && \
    echo "export SSL_CERT_FILE=$(python3 -m certifi)" >> ~/.profile

# # 安装 Miniconda 到 ~/.conda
# RUN mkdir -p /home/${USER_NAME}/.conda && \
#     wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /home/${USER_NAME}/.conda/miniconda.sh && \
#     bash /home/${USER_NAME}/.conda/miniconda.sh -b -u -p /home/${USER_NAME}/.conda && \
#     rm /home/${USER_NAME}/.conda/miniconda.sh

# ENV PATH="/home/${USER_NAME}/.conda/bin:$PATH"

# # 配置 pip 镜像
# RUN /home/${USER_NAME}/.conda/bin/pip install --upgrade pip

# enable ssh key login
#RUN mkdir /home/ubuntu/.ssh && \
#    echo "Your ssh key" > /home/ubuntu/.ssh/authorized_keys

RUN git clone https://github.com/ra4ing/dotfile.git /home/${USER_NAME}/.dotfile
RUN cd /home/${USER_NAME}/.dotfile && ./setup.sh && ./setup_ssh.sh

ENV SHELL=/bin/zsh

CMD ["zsh"]

EXPOSE 22
