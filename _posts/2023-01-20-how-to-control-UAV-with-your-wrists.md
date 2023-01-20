# How I controlled UAV with wrist movement? 

Last spring, I was developing a system for intuitive UAV control. 

In short, I thought that controlling UAV with certain body motions would be easier than using 
classical RC (radio-controlled) joystick. 

To detect body motions, I have used simplebaselines neural network and simple web-cam. 
UAV control was implemented as simple PID cascade control. 

With simple arithmetics, I've turned wrist movement in certain image zones, to 
the UAV commands: 
 * height +/- 
 * roll +/-
 * pitch +/-
 * yaw +/- 

All image-plane pixel movements were scaled to the range of [-1, 1] to emulate joystick 
output. 

I turns out, I was wrong, however - I've developed system prototype using tools as: 
 * docker 
 * tmuxinator 
 * ROS 
 * gazebo 

which made user-study feasable and possible. 

I divided system in multiple parts: 
 * user-perception part --> hpe_ros_cont
 * uav-control part --> uav_cont 

Both of those parts had corresponding docker image (container) which were used to 
enable faster development and deployment:
 * [hpe_ros_cont](https://github.com/larics/docker_files/blob/master/ros-melodic/hpe_ros/Dockerfile) 
 * [uav_cont](https://github.com/larics/docker_files/tree/master/ros-melodic/mmuav_ros) 

Tmuxinator configs used for runing multiple experiments for RC joystick and HPC (human pose control) 
comparison are:
 * [HPC experiment](https://github.com/larics/docker_files/blob/master/tmux-sessions/hpe_experiment.yml) 
 * [RC experiment](https://github.com/larics/docker_files/blob/master/tmux-sessions/rc_experiment.yml) 

I would run them as: 
```
tmuxinator start hpe_experiment user_id=<user_id> run=<num_run>
```

Tmuxinator runs following: 
 * UAV simulation/control 
 * Human pose control 
 * RTSP streams (for FPV stream) 

Docker configuration for initializing docker containers is: 
```
docker run -it \
    --env="DISPLAY=$DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --volume="/dev:/dev" \
    --net=host \
    --privileged \
    --gpus all \
    --name hpe_ros_cont \
    hpe_ros_img:latest
```

Whereas most important variables are: 
 * `--net=host` --> docker container shares network with HOST pc, which enables message exchange between containers
 * `--privileged` --> docker container has sudo privileges on HOST pc (mainly for device access) 
 * `--gpus all` --> docker has access to GPU 
 * `--volume="/dev:/dev"` --> docker has access to all devices (USBs, CAMs, etc...) 

Detailed explanation of the whole system can be found [here](https://ieeexplore.ieee.org/document/9954775/). 

This work shows how to efficiently and easily use docker and tmuxinator in combination for experimental purposes. 

**Please have in mind that providing privileges to the docker container can be potentially harmful. Make sure 
Dockerfile contents are harmless before providing such privileges.** 
