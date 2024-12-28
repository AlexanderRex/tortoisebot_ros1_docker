FROM ros:noetic-ros-base

RUN apt-get update && apt-get upgrade -y && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /catkin_ws/src

WORKDIR /catkin_ws/src

CMD ["bash"]
