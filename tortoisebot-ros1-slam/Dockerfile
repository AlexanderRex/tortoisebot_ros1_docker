FROM nvidia/cuda:11.7.1-base-ubuntu20.04

# Set environment variables to skip timezone selection
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

# Minimal setup and install ROS Noetic and necessary build tools
RUN apt update \
    && apt install -y locales lsb-release curl gnupg2 git tzdata \
    && ln -fs /usr/share/zoneinfo/Etc/UTC /etc/localtime \
    && dpkg-reconfigure --frontend noninteractive tzdata \
    && locale-gen en_US en_US.UTF-8 && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 \
    && echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list \
    && curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - \
    && apt update \
    && apt install -y --no-install-recommends ros-noetic-desktop-full \
    && apt install -y ros-noetic-rosbridge-suite ros-noetic-slam-gmapping ros-noetic-dwa-local-planner ros-noetic-map-server ros-noetic-move-base ros-noetic-teleop-twist-keyboard \
    && echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc \
    && apt install -y python3-rosdep python3-rosinstall python3-catkin-tools python3-rosinstall-generator python3-wstool build-essential ninja-build stow iputils-ping netcat-openbsd\
    && rosdep init \
    && rosdep update

RUN mkdir -p ~/cartographer_ws/src
WORKDIR /root/cartographer_ws/src
RUN wstool init . \
    && wstool merge -t . https://raw.githubusercontent.com/cartographer-project/cartographer_ros/master/cartographer_ros.rosinstall \
    && wstool update -t .

WORKDIR /root/cartographer_ws
RUN rosdep install --from-paths src --ignore-src --rosdistro=noetic -y --skip-keys="libabsl-dev"

WORKDIR /root/cartographer_ws/src/cartographer
RUN ./scripts/install_abseil.sh

WORKDIR /root/cartographer_ws
RUN /bin/bash -c "source /opt/ros/noetic/setup.bash && catkin_make_isolated --install --use-ninja"

RUN mkdir -p ~/simulation_ws/src \
    && git clone https://github.com/AlexanderRex/tortoisebot.git ~/simulation_ws/src/tortoisebot \
    && git clone https://github.com/AlexanderRex/course_web_dev_ros.git ~/simulation_ws/src/course_web_dev_ros

RUN bash -c "find ~/simulation_ws/src/tortoisebot/* -maxdepth 0 -type d ! \( -name 'tortoisebot_slam' -o -name 'tortoisebot_description' -o -name 'tortoisebot_firmware' -o -name 'tortoisebot_navigation' \) -exec rm -rf {} +"


WORKDIR /root/simulation_ws
RUN /bin/bash -c "source /opt/ros/noetic/setup.bash && catkin_make"

RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc && \
    echo "source ~/cartographer_ws/install_isolated/setup.bash --extend" >> ~/.bashrc && \
    echo "source ~/simulation_ws/devel/setup.bash --extend" >> ~/.bashrc

CMD ["/bin/bash"]
