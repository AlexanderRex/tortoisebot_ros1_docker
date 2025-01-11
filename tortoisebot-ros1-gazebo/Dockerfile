FROM nvidia/cuda:11.7.1-base-ubuntu20.04

# Set environment variables to skip timezone selection
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

# Minimal setup and install ROS Noetic
RUN apt update \
    && apt install -y locales lsb-release curl gnupg2 git tzdata \
    && ln -fs /usr/share/zoneinfo/Etc/UTC /etc/localtime \
    && dpkg-reconfigure --frontend noninteractive tzdata \
    && locale-gen en_US en_US.UTF-8 && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 \
    && echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list \
    && curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - \
    && apt update \
    && apt install -y --no-install-recommends ros-noetic-desktop-full \
    && echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc \
    && apt install -y python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential \
    && rosdep init \
    && rosdep update

# Create working directory and clone the project repository
RUN mkdir -p ~/simulation_ws/src \
    && git clone https://github.com/AlexanderRex/tortoisebot.git ~/simulation_ws/src/tortoisebot

# Remove all folders except tortoisebot_gazebo and tortoisebot_description
RUN find ~/simulation_ws/src/tortoisebot/* -maxdepth 0 -type d ! \( -name 'tortoisebot_gazebo' -o -name 'tortoisebot_description' \) -exec rm -rf {} +

# Build the workspace
RUN /bin/bash -c "source /opt/ros/noetic/setup.bash && cd ~/simulation_ws && catkin_make"

# Source the setup.bash
RUN echo "source ~/simulation_ws/devel/setup.bash" >> ~/.bashrc