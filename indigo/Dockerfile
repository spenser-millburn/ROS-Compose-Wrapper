# Start from a base image of Ubuntu 14.04
FROM nvidia/opengl:1.0-glvnd-runtime-ubuntu14.04

ENV NVIDIA_DRIVER_CAPABILITIES ${NVIDIA_DRIVER_CAPABILITIES},display

RUN apt-get update && apt-get install -y --no-install-recommends \
        mesa-utils && \
    rm -rf /var/lib/apt/lists/*

# Setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list

# Setup keys
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

# Update the package repository and install the required packages
RUN apt-get update && apt-get install -y \
  ros-indigo-desktop-full \
  ros-indigo-image-transport \
  ros-indigo-image-transport-plugins \
  ros-indigo-pcl-ros \
  ros-indigo-libg2o

ENV ROS_DISTRO indigo
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
