# panda_simulator

## Package Overview
Repository that hosts all the required ROS packages for simulating the Franka Emika Panda Arm in Gazebo. libfranka is also required, but not included here. 

Please enure you have these dependencies:

```sh
sudo apt install ros-$ROS_DISTRO-gazebo-ros-pkgs ros-$ROS_DISTRO-gazebo-ros-control ros-$ROS_DISTRO-libfranka
```

## Installation and Usage
1. `git clone https://git.uwaterloo.ca/vrajendr/panda_simulator.git` (into your catkin workspace e.g. catkin_ws/src folder) or add as a submodule `git submodule add https://git.uwaterloo.ca/vrajendr/panda_simulator.git`
2. `git submodule update --init --recursive`
3. `catkin build` from your catkin workspace folder
4. remember to `source devel/setup.bash` 
5. `roslaunch panda_simulator_support panda_simulation.launch` (this will start up Gazebo and RVIZ and will allow you to do planning etc. with the robot)

## Notes

Default controllers used are `hardware_interface/PositionJointInterface`, if you need effort controllers use the following launch command:
```sh
roslaunch panda_simulator_support panda_simulation.launch controller_type:=effort
```
---
https://github.com/erdalpekel/panda_simulation/issues/9

Ignore the following error when launching in position mode:
`[ERROR] [1584377149.867905243, 0.071000000]: No p gain specified for pid. Namespace: /gazebo_ros_control/pid_gains/panda_joint1`
As noted by the original developer of the package, "The reason why we are not specifying those parameters is that we want to use the Gazebo internal joint position controller, not the PID controller."

---

In the panda_simulation.launch launch file, the argument gripper_center is set to `true`. This creates a new frame at the tip of the robot end effector that can be used while doing motion planning with MoveIt! (instead of using panda_link8 which is offset from the actual gripper joints)

Usage is like below:
```
panda_arm = moveit_commander.MoveGroupCommander("panda_arm")
panda_arm.set_end_effector_link("panda_gripper_center")
```

---

# To-Do

- [x] Update dynamics parameters using http://diag.uniroma1.it/~gaz/panda2019.html [COMPLETED, new dynamic parameters seem to be more accurate]
- [ ] Make it easy to move back and forth from simulation to the real robot. I think it mainly needs changes to the controller list in the MoveIt! config.
- [ ] Update repo with RoboHub Panda on table model + related MoveIt! Config.