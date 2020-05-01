FROM yingjun/dockergui:latest

MAINTAINER YingJun<wandy1208@gmail.com>

#########################################
##        ENVIRONMENTAL CONFIG         ##
#########################################

# Set environment variables

# User/Group Id gui app will be executed as default are 99 and 100
ENV USER_ID=99
ENV GROUP_ID=100

ENV EDGE="0"

# Gui App Name default is "GUI_APPLICATION"
ENV APP_NAME="Ride"

# Default resolution, change if you like
ENV WIDTH=1280
ENV HEIGHT=720

# Use baseimage-docker's init system
CMD ["/sbin/my_init"]

# Change source to tsinghua
# RUN mv /etc/apt/sources.list /etc/apt/sources.list.copy

# RUN echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse' >> /etc/apt/sources.list


# Install packages needed for app
RUN export DEBCONF_NONINTERACTIVE_SEEN=true DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -y build-essential\
                    python3-pip \
                    curl \
                    wget \
                    bash \
                    git \
                    firefox
                    # libsdl2-mixer-2.0-0 \
                    # libsdl2-image-2.0-0 \
                    # libsdl2-2.0-0 && \
    # pip3 install robotframework-ride


# Install ImageMagick
RUN apt-get update && \
    # apt upgrade -y && \
    apt-get build-dep -y imagemagick && \
    wget https://www.imagemagick.org/download/ImageMagick.tar.gz && \
    tar xf ImageMagick.tar.gz && \
    cd ImageMagick-7* && \
    ./configure && \
    make && \
    make install && \
    make clean && \
    ldconfig /usr/local/lib
    # rm /etc/apt/sources.list.d/trusty-copies.list

#########################################
##    REPOSITORIES AND DEPENDENCIES    ##
#########################################
# RUN echo 'deb http://archive.ubuntu.com/ubuntu trusty main universe restricted' > /etc/apt/sources.list && \
#     echo 'deb http://archive.ubuntu.com/ubuntu trusty-updates main universe restricted' >> /etc/apt/sources.list && \

# Install packages needed for app

# RUN export DEBCONF_NONINTERACTIVE_SEEN=true DEBIAN_FRONTEND=noninteractive && \
#     apt-get update && \
#     apt-get install -y build-essential \
#                        python3 \
#                     #    python3-dev \
#                        python3-pip \
#                     #    python3-lxml \
#                        curl \
#                        bash \
#                        firefox \
#                        git \
#                        wget


#安装中文语言包
RUN apt-get install -y language-pack-gnome-zh-hans && \
    apt-get install -y ttf-wqy-zenhei

# 处理中文问题
ENV LANG=zh_CN.UTF-8

# 处理时区问题
RUN echo "Asia/shanghai" > /etc/timezone
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime

#########################################
##          Install GTK+ 3.0           ##
#########################################
RUN apt-get install -y libgtk-3-dev && \
    apt-get install -y build-essential pkg-config devhelp glade libglade2-dev

# Change PIP source
# RUN mkdir ~/.pip && \
#     touch ~/.pip.conf && \
#     echo "[global]" >> ~/.pip/pip.conf && \
#     echo "index-url = https://pypi.tuna.tsinghua.edu.cn/simple/ " >> ~/.pip/pip.conf && \
#     echo "[install]" >> ~/.pip/pip.conf && \
#     echo "trusted-host = pypi.tuna.tsinghua.edu.cn" >> ~/.pip/pip.conf
#########################################
##          Install app components     ##
#########################################
RUN pip3 install --upgrade pip && \
    # pip3 install -U -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-18.04 wxPython==4.1.0 && \
    pip3 install -U paramiko \
                    wxPython==4.1.0 \
                    robotframework \
                    selenium \
                    robotframework-seleniumlibrary \
                    robotframework-ride==1.7.4.2

#########################################
##          GUI APP INSTALL            ##
#########################################

# WORKDIR /nobody
# RUN git clone  -b release/1.7.4.2 https://github.com/robotframework/RIDE.git /nobody/RIDE && \
#     mkdir -p /etc/my_init.d && \
#     echo 'admin ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
WORKDIR /nobody/RIDE
# RUN /usr/bin/python setup.py install
ADD firstrun.sh /etc/my_init.d/firstrun.sh
RUN chmod +x /etc/my_init.d/firstrun.sh

# Copy X app start script to right location
COPY startapp.sh /startapp.sh

#########################################
##         EXPORTS AND VOLUMES         ##
#########################################

ENV HOME /nobody
VOLUME ["/config", "/robot"]
EXPOSE 3389 8080