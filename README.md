This repository is intended to prepare the minimum ADE environment which contains the following items:
- ROS2 humble
- Nvidia driver

## prerequisites
- docker engine
- nvidia contrainer toolkit
``` bash
cd repo-root/humble/
docker build -t ros2-humble-cuda-opengl .
cd repo-root/ade/
docker build -t ade-ros2-humble-cuda-opengl .
```

## Start the ADE
```bash
cd repo-root/adehome
ade start
ade enter
```

## Usefull links:
- https://gitlab.com/ApexAI/minimal-ade
- https://gitlab.com/ApexAI/ade-nvidia-cudagl
- https://gitlab.com/ApexAI/ros2-ade





