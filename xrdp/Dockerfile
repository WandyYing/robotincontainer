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

# Install ImageMagick
RUN apt-get update && \
    apt-get install -y wget \
    # apt upgrade -y && \
    apt-get build-dep -y imagemagick && \
    wget https://www.imagemagick.org/download/ImageMagick.tar.gz && \
    tar xf ImageMagick.tar.gz && \
    cd ImageMagick-7* && \
    ./configure && \
    make && \
    make install && \
    make clean && \
    ldconfig /usr/local/lib && \
    rm /etc/apt/sources.list.d/trusty-copies.list


RUN mv /etc/apt/sources.list /etc/apt/sources.list.copy

# RUN echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse' >> /etc/apt/sources.list && \
#     echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse' >> /etc/apt/sources.list && \ 
#     echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse' >> /etc/apt/sources.list && \ 
#     echo 'deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse' >> /etc/apt/sources.list

## 
RUN apt-get install -y python3-pip \
                    curl \
                    bash \
                    git
                    # libsdl2-mixer-2.0-0 \
                    # libsdl2-image-2.0-0 \
                    # libsdl2-2.0-0 && \
    # pip3 install robotframework-ride




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

# # Install ImageMagick
# RUN echo "deb-src http://mirrors.163.com/ubuntu/ bionic main restricted" | tee /etc/apt/sources.list.d/trusty-copies.list && \
#     apt update && \
#     # apt upgrade -y && \
#     apt build-dep -y imagemagick && \
#     wget https://www.imagemagick.org/download/ImageMagick.tar.gz && \
#     tar xf ImageMagick.tar.gz && \
#     cd ImageMagick-7* && \
#     ./configure && \
#     make && \
#     make install && \
#     make clean && \
#     ldconfig /usr/local/lib && \
#     rm /etc/apt/sources.list.d/trusty-copies.list

# RUN python3 -m pip install https://extras.wxpython.org/wxPython4/extras/linux/gtk2/centos-7/wxPython-4.0.7.post2-cp36-cp36m-linux_x86_64.whl

#安装中文语言包
RUN sudo apt-get install -y language-pack-gnome-zh-hans && \
    sudo apt-get install -y ttf-wqy-zenhei

# 处理中文问题
ENV LANG=zh_CN.UTF-8

# 处理时区问题
RUN echo "Asia/shanghai" > /etc/timezone
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime


#########################################
##          Install app components     ##
#########################################
RUN pip3 install --upgrade pip && \
    pip3 install paramiko robotframework && \
    pip3 install -U -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-18.04 wxPython
    
# RUN pip3 install wxpython && \
# RUN pip3 install -U robotframework \
#                     selenium \
#                     robotframework-seleniumlibrary

#########################################
##          GUI APP INSTALL            ##
#########################################

WORKDIR /nobody
RUN git clone  -b release/1.7.4.2 https://github.com/robotframework/RIDE.git /nobody/RIDE && \
    mkdir -p /etc/my_init.d && \
    echo 'admin ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
WORKDIR /nobody/RIDE
RUN /usr/bin/python setup.py install
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