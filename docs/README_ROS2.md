# Running ROS2 on a Raspberry Pi 4B

It requires the 64-bit version of Raspberry Pi OS and Docker
installed from docker.com directly.

[ROS2 on Raspberry Pi OS](https://docs.ros.org/en/foxy/How-To-Guides/Installing-on-Raspberry-Pi.html)

## Build the docker image

1. Start with a Raspberry Pi 4B
```
Hardware    : BCM2835
Revision    : c03112
Model       : Raspberry Pi 4 Model B Rev 1.2
```
2. Install Raspberry Pi OS Bullseye and docker Server 23.0.3
3. cd to auv_delivery_support/ros2ws
4. Build the image
```
docker build -t delivery -f Dockerfile.delivery .
```
5. Add the X150 as /dev/ttyUSB0
6. Start the container
```
docker run -it --rm \
    --name delivery \
    --device /dev/ttyUSB0 \
    delivery \
    /bin/bash

source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch delivery delivery.launch.py
```
