FROM nvcr.io/nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04

LABEL maintainer="ery.eden@gmail.com"

RUN apt update

# Install OpenGL related packages for cuda: https://qiita.com/ntrlmt/items/c0a807aeff83eea7be24
RUN apt-get update && apt-get install -y --no-install-recommends \
        pkg-config \
        libglvnd-dev \
        libgl1-mesa-dev \
        libegl1-mesa-dev \
        libgles2-mesa-dev

# Fix locale
RUN apt-get update && \
    echo 'Etc/UTC' > /etc/timezone && \
    ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get install -y \
        sudo \
        locales \
        tzdata
RUN locale-gen en_US.UTF-8; dpkg-reconfigure -f noninteractive locales
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV LC_ALL en_US.UTF-8

WORKDIR /ros2_humble

# Add the ROS apt repository, then install away
# Adapted from https://index.ros.org/doc/ros2/Installation/Humble/Linux-Development-Setup/
RUN apt-get install -y \
        curl \
        gnupg2 && \
    curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu jammy main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null && \
    apt-get update && \
    apt-get install -y  \
        bash-completion \
        build-essential \
        cmake \
        git \
        libbullet-dev \
        python3-colcon-common-extensions \
        python3-flake8 \
        python3-pip \
        python3-pytest-cov \
        python3-rosdep \
        python3-setuptools \
        python3-vcstool \
        acl \
        libacl1-dev \
        libssl3 \
        vim \
        wget && \
    apt-get install --no-install-recommends -y \
        libasio-dev \
        libtinyxml2-dev \
        libcunit1-dev && \
    python3 -m pip install -U \
        argcomplete \
        colcon-mixin \
        flake8-blind-except \
        flake8-builtins \
        flake8-class-newline \
        flake8-comprehensions \
        flake8-deprecated \
        flake8-docstrings \
        flake8-import-order \
        flake8-quotes \
        pytest-repeat \
        pytest-rerunfailures \
        pytest && \
    mkdir -p /ros2_humble/src && \
    wget https://raw.githubusercontent.com/ros2/ros2/humble/ros2.repos && \
    vcs import src < ros2.repos

RUN sudo rosdep init

# Note. rosdep update and rosdep install shall not be running in sudo mode
RUN rosdep update && \
    rosdep install --from-paths src --ignore-src --rosdistro humble -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers" && \
    rm -rf /ros2_humble

# Install ros-github-scripts
RUN git clone https://github.com/ros-tooling/ros-github-scripts
RUN pip3 install ./ros-github-scripts && \
    rm -rf /ros-github-scripts

# Install ros2 humble desktop
RUN apt-get install -y ros-humble-ros-base

# Install CCACHE
RUN apt install -y ccache && \
    rm -rf /var/lib/apt/lists/*
