FROM dorowu/ubuntu-desktop-lxde-vnc

# Install packages needed for app
RUN apt-get update && \
    apt-get remove -y vim-common && \
    apt-get install -y vim \
                       curl \
                       wget \
                       build-essential \
                       python3 \
                       python3-pip \
                       python3-dev \
                       python3-lxml \
                       libjpeg8-dev \
                       zlib1g-dev \
                       libfreetype6-dev \
                       libaio-dev \
                       fcitx \
                       fcitx-googlepinyin \
                       fcitx-module-cloudpinyin \
                       fcitx-sunpinyin \
                       language-pack-gnome-zh-hans  \
                       ttf-wqy-zenhei

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
RUN pip3 install --upgrade pip3
RUN pip3 install wxpython && \
    pip3 install -U robotframework \
                    selenium \
                    robotframework-seleniumlibrary \
                    https://github.com/robotframework/RIDE/archive/master.zip


COPY startapp.sh /startapp.sh
VOLUME ["/config", "/robot"]
EXPOSE 3389 8080