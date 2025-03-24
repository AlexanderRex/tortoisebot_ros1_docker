
# Tortoisebot docker for simulation

This package needed to build and run containers for tortoise bot in simulation

## How to run:

Move to root of tortoisebot_ros1_docker directory and then

```
docker compose up
```

after this action you will have ready to use containers

Next step is to connect to each container and do the following:

### tortoisebot-ros1-gazebo

```
roslaunch tortoisebot_gazebo tortoisebot_playground.launch
```

### tortoisebot-ros1-slam

```
roslaunch tortoisebot_slam mapping.launch
```

### tortoisebot-ros1-waypoints

```
rosrun course_web_dev_ros tortoisebot_action_server.py
```

### tortoisebot-ros1-webapp

```
cd ~/simulation_ws/src/tortoisebot_webapp
python3 -m http.server 7000
```

```
roslaunch course_web_dev_ros tf2_web.launch
```

```
roslaunch course_web_dev_ros web.launch
```

To connect to ros server

```
ws://localhost:9090
```