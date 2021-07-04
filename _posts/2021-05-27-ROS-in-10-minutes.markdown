# ROS in 10 minutes


From my experince, conceptual understanding is far more important in beggining than complete 
understanding of all the technicalities. In the end, you can google technicalities, while gathering 
conceptual understanding sometimes takes years to develop. In this post, I'll try to explain
basic ROS concepts which are fundamental for any robotic application that uses ROS and for any developer/programmer/engineer 
that's going to use ROS. 

# What is ROS? 

Robot operating system (ROS) is defined as set of libraries and tools for robotic application development. 
From my perspective, core of the robot control is information exchange. We mostly want to control some 
electro-mechanical system. Almost everything in control theory revolves around feedback loop. Feedback 
loop provides simple, yet efficient way to compare reference value with measured value and based on 
difference of those two (error) undertake some action to minimize such error. Vast variety of 
different sensors (basically mesurement devices), computing devices, actuators and algorithms 
enable us to develop complex robotic systems. ROS provides tooling and framework which enables us to do so. 

Official definition of ROS is: 
```
ROS is an open-source, meta-operating system for your robot. It provides the services you would expect from an operating system,
including hardware abstraction, low-level device control, implementation of commonly-used functionality, message-passing between processes, and package management. 
It also provides tools and libraries for obtaining, building, writing, and running code across multiple computers.
```

**ROS is a framework that gives us tools to efficiently exchange and process informations between different hardware/software systems.**

# ROS concepts breakdown

## ROS MASTER 

ROS Master provides naming and registration services to the rest of the nodes in the ROS system. 
It enables various nodes to discover each other, and to communicate with each other peer-to-peer. 
ROS Master also provides parameter server which gives us ability to upload/get/set/change parameters
to extend code modularity (e.g. you can have controller gains loaded to parameter server, and switch 
them accordingly depending on the robot used). ROS Master uses XMLRPC API which is used to store and retrieve
information. 

## ROS networking 

ROS communicates over the network. It's required to satisfy certain requirements of the network configuration: 
 * There must be complete, bi-directional connectivity between all pairs of machines, on all ports. 
 * Each machine must advertise itself by a name that other machines can resolve (2 machines CAN'T have same name) 

Following environment variables basically define networking setup: 
 * `ROS_MASTER_URI` : ip address of a machine that runs master proces (e.g. ROS MASTER), should be same across machines/robots 
 * `ROS_HOSTNAME`: hostname of the machine where we want to run/execute ROS nodes
 * `ROS_IP`: ip of the machine where we want to run/execute ROS nodes

It's enough to define `ROS_IP` or `ROS_HOSTNAME`. In some advanced scenarios it's required to use multiple masters, in 
case if needed, you can check [this](https://github.com/AIS-Bonn/nimbro_network) or [this](http://wiki.ros.org/multimaster_fkie).  

[ROS Network Setup](http://wiki.ros.org/ROS/NetworkSetup) 

[ROS on multiple machines](http://wiki.ros.org/ROS/Tutorials/MultipleMachines)

## Nodes 

ROS node is a process that performs a computation. You can think of a ROS node as a minimal functional 
part of your robotic application. For example, if we want to develop object detection with some neural network we could
have following nodes: 
 * camera_node --> gathering images from camera sensor 
 * object_detection_node --> preforms inference on captured image 

Or if we have some UAV which we want to control, we could have following nodes: 
 * hw_driver --> transforms high-level references such as rotor speed into low-level commands which are passed to brushless DC motors 
 * attitude_controller_node --> implementation of cascaded PID control loop attitude control (z axis) 
 * position_controller_node --> implementation of cascaded PID control loop for position control (x,y axis)  

So you get the idea, nodes are sub-parts of your system that should be closely coupled with some robot functionality or a system. 

Nodes are written mostly in [cpp](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29) or in [python](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28python%29). 

[Official documentation about nodes](http://wiki.ros.org/Nodes) 

How to use `rosnode cli`? 
If you installed ROS, and you want to check which commands are available to check on nodes, run following command: 
```
rosnode --help 
```

and you'll find out all available commands which could be of help. Such as `rosnode list` (list all currently active nodes), 
`rosnode info <node_name>` (find out more about active node). Everything is available at the tip of your fingers, **just use available CLI**. :)

## Topics 

ROS topics are used for continous data exchange. Every node can register publishers/subscribers on certain topic, depending on node functionallity. 

For example, previously mentioned nodes in object detection example are: 
 * camera_node **publishes** image in the form of predefined message `sensor_msgs/Image` on `/camera/image` topic 
 * object_deteciont_node **subscribes** on the `/camera/image` topic 

Subscribers read data from certain topic and pass that data to defined callback methods (callback method invokes inference on image in case of object_detection_node). 
Publishers publish data on certain topic with defined frequency and with corresponding buffer. 

Power of ROS is contained in the ROS msgs which give us powerful tool for exploiting existing data structures with typed fields that are used quite often. 
You have at your disposal multiple predefined msgs such as [sensor_msgs](http://wiki.ros.org/sensor_msgs) which have structures such as LaserScan, Image, Imu etc.. 
There are also following predefined msg packages such as [nav_msgs](https://www.google.com/search?channel=fs&client=ubuntu&q=nav_msgs), or [geometry_msgs](http://wiki.ros.org/geometry_msgs) 
and [std_msgs](http://wiki.ros.org/std_msgs). 

If you want to find out what you can do with topics, use following command: 
```
rostopic --help
```

After that you can find out which nodes publish/subscribe on certain topic with `rostopic info <topic_name>`, you can echo published msg with `rostopic echo <topic_name>` and
at which frequency with `rostopic hz <topic_name>`.

Same is for ROS msgs which has `rosmsg` as command for CLI. You can find out available commands with `rosmsg --help`, and explore further certain msg 
type with `rosmgs show <msg_name>`... **Use available --help command in CLI**

## Services 

ROS services are used for asynchronous data exchange. For example you want to turn on lights at some trigger event, you'll invoke client which 
will send request to turn on the lights to `/srv/turn_lights` service. Services are quite similar to topics except they're: 
 * asynchronous  
 * client-server architecture 
 * have response/request definition in srv type 

So for example `TurnLights.srv` could look like something like this: 
```
req
---
int LightIntensity %[0-100] percentage of illumination 
---
res
bool success 
```

If we send request to change light intensity we should get response which gives us information if our service call has been successful. 

If you want to explore more about services, use available CLI, and type `rosservice --help` to find out about all available commands. 

Same as with msgs, there is wide variety of predefined service structures such as [std_srvs](http://wiki.ros.org/std_srvs) 

## Actions 

Actions are used when we want to execute some long-lasting, complex action with our robot/system. Every 
action is set of a ROS msgs, and possibly multiple ROS nodes. We need to clearly define `goal, feedback and result`. 

For thought experiment we can create action server which will turn on lights when we detect 3 birds on our image stream. 
For such experiment we need ros nodes for following: 
 * camera_node --> capture images from camera 
 * object_detection_node --> detects objects on image 
 * action_server --> invokes sequence of certain tasks to execute certain action

**Goal** is something we send to our system to command execution of some long-lasting operation. E.g we can pass 
as a goal timeout in which we want to detect those 3 birds. In our **feedback** we want to know 
how many birds are currently detected, and in our **result** we could have boolean if action succeded/failed. 

Action would look something like this: 
```
goal
---
int timeout_s

feedback
---
int n_detected_birds 
bool lights_on 

result
---
bool suceeded
```

Actions are more complex topic, but this brief explanation should be enough to figure out when is right time for 
employing it.  

[actionlib](http://wiki.ros.org/actionlib)  
[python-writing a simple action server](http://wiki.ros.org/actionlib_tutorials/Tutorials/Writing%20a%20Simple%20Action%20Server%20using%20the%20Execute%20Callback%20%28Python%29)  
[cpp-writing a simple action server](http://wiki.ros.org/actionlib_tutorials/Tutorials/SimpleActionServer%28ExecuteCallbackMethod%29)  


## ROS data structure 

ROS has quite simple basic data structure. There is [ROS workspace](http://wiki.ros.org/catkin/Tutorials/create_a_workspace) which contains all custom  
[ROS packages](http://wiki.ros.org/ROS/Tutorials/CreatingPackage) that are used in some robotic application. ROS workspace contains of following folders:  
 * build -->  CMake is invoked to build the catkin packages in the source space  
 * devel --> where built targets are placed prior to being installed  
 * install --> once targets are built, they can be installed into the install space (in case we want to delete source code), doesn't have to be in workspace (defined by `CATKIN_INSTALL_PREFIX`) 
 * src --> contains source code of catkin packages (place where you develop your ROS nodes)  

Each ROS package has following data structure:  
 * CMakeLists.txt --> file that defines how to build ROS node, more info available on this [link](http://wiki.ros.org/catkin/CMakeLists.txt)  
 * include --> headers for cpp ROS nodes, and anything else that's included in your code
 * config --> configuration files, dynamic reconfigure files  
 * launch --> [roslaunch](http://wiki.ros.org/roslaunch) files (define which nodes to run with which arguments)  
 * src --> contains source code, rospy and roscpp nodes  


# Further resources 

* [Installing and configuring your ROS Environment](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment)  
* [ROS Tutorials](http://wiki.ros.org/ROS/Tutorials)  
* [roscpp overview](http://wiki.ros.org/roscpp/Overview)  
* [rospy overview](http://wiki.ros.org/rospy/Overview)  
* [ROS documentation](http://wiki.ros.org/Documentation)   

# How to start? 

Read documentation from ROS wiki, especially roscpp & rospy overview. Skim through begginer ROS tutorials to gain conceptual understanding of different topics. 
Google for existing ROS packages and/or explore source code of those that are often used. Pay attention to [Programming for the Robot Operating System](https://www.fer.unizg.hr/predmet/pzros)
because it covers everything neccessary for learning basics. Check this [course](https://rsl.ethz.ch/education-students/lectures/ros.html) from ETH zurich. 
Check this [example](https://github.com/leggedrobotics/darknet_ros) of good ROS package for object detection. [Universal robot](https://github.com/ros-industrial/universal_robot) 
ROS package for robotic manipulation. [Package](https://github.com/ros-drivers/usb_cam) for USB ROS camera. Explore Github issues from package you're going to use. 

And in the end, if something doesn't work, it's probably because you've haven't anticipated something. ROS works 99.9999% of the time as expected. 
Enjoy watching robots move on your command :)  
