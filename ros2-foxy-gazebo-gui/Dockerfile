# ros-foxy-desktop-gui

# docker build -t ros-foxy-gazebo-gui .
# xhost +si:localuser:root
# docker run --runtime=nvidia -ti --rm -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix ros-foxy-gazebo-gui

FROM nvidia/opengl:1.0-glvnd-runtime-ubuntu20.04

ENV NVIDIA_DRIVER_CAPABILITIES ${NVIDIA_DRIVER_CAPABILITIES},display

# Configure keyboard options
# See: https://github.com/phusion/baseimage-docker/issues/342#issuecomment-281315763
COPY ./keyboard /etc/default/keyboard

# setup timezone
RUN echo 'Etc/UTC' > /etc/timezone && \
    ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata \
    mesa-utils \
    gnupg2 \
    curl \
    lsb-release \
    build-essential \
    python3-pip \
    && pip3 install -U argcomplete \
    && rm -rf /var/lib/apt/lists/*

ENV LANG=en_US.UTF-8 LANGUAGE=en_US:en LC_ALL=en_US.UTF-8

# setup keys and sources.list
RUN sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list' \
    && cat /etc/apt/sources.list.d/ros2-latest.list
RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add -

# install ros foxy desktop full
RUN apt-get update && apt-get install -y ros-foxy-desktop

# bootstrap env
RUN echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc

CMD ["bash"]