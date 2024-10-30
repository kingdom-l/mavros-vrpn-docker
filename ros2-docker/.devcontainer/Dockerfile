FROM osrf/ros:humble-desktop-full

SHELL [ "/bin/bash" , "-c" ]

# Upgrade all packages
RUN sudo apt update && sudo apt upgrade -y

# Install essential packages
RUN sudo apt install -y wget

# Create overlay workspace
WORKDIR /root/ros_ws/src
COPY packages.repos .

RUN vcs import < packages.repos; \
    cd ..; \
    rosdep install --from-paths src --ignore-src -r -y; \
    source /opt/ros/${ROS_DISTRO}/setup.bash; \
    colcon build;

RUN apt update; apt install -y iputils-ping \ 
    net-tools \
    ros-humble-mavros \
    ros-humble-mavros-extras \
    && sudo /opt/ros/humble/lib/mavros/install_geographiclib_datasets.sh

# Add sourcing ROS setup.bash to .bashrc
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc