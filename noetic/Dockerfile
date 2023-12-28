# Start from a base image of Ubuntu 14.04
FROM nvidia/opengl:1.0-glvnd-runtime-ubuntu20.04

# nvidia environment
ENV NVIDIA_DRIVER_CAPABILITIES ${NVIDIA_DRIVER_CAPABILITIES},display

# setup timezone
RUN echo 'Etc/UTC' > /etc/timezone && \
    ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata && \
    rm -rf /var/lib/apt/lists/*

# setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

RUN apt-get update && apt-get install -y --no-install-recommends mesa-utils curl gnupg gnupg2 gnupg1 && rm -rf /var/lib/apt/lists/*

# Setup keys
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

# setup sources.list
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu focal main" > /etc/apt/sources.list.d/ros-latest.list'

# Update the package repository and install the required packages
RUN apt-get update && apt-get install -y ros-noetic-desktop-full 

RUN  apt install -y python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential



ENV ROS_DISTRO noetic
# Setup the environment variables to fix QT bug
ENV QT_X11_NO_MITSHM 1

# Source the ROS environment setup script
RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> /root/.bashrc

SHELL ["/bin/bash", "-c"]
# Setup entrypoint
RUN echo -e "#!/bin/bash \n\
set -e \n\
source \"/opt/ros/$ROS_DISTRO/setup.bash\" -- \n\
exec \"\$@\" " > entrypoint.sh

RUN chmod +x entrypoint.sh
ENTRYPOINT [ "/entrypoint.sh" ]
