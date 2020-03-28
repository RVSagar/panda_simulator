# panda_simulator

Repository that hosts all the required packages for simulating the Franka Emika Panda Arm.

1. `git clone https://git.uwaterloo.ca/vrajendr/panda_simulator.git` (into your catkin workspace e.g. catkin_ws/src folder) or add as a submodule `git submodule add https://git.uwaterloo.ca/vrajendr/panda_simulator.git`
2. `catkin build`
3. remember to `source devel/setup.bash` 
4. `roslaunch panda_simulator panda_simulation.launch` (this will start up Gazebo and RVIZ and will allow you to do planning etc. with the robot)

Notes:

https://github.com/erdalpekel/panda_simulation/issues/9

Ignore the following error when launching:
`[ERROR] [1584377149.867905243, 0.071000000]: No p gain specified for pid. Namespace: /gazebo_ros_control/pid_gains/panda_joint1`
As noted by the original developer of the package, "The reason why we are not specifying those parameters is that we want to use the Gazebo internal joint position controller, not the PID controller."

---

In the panda_simulation.launch launch file, the argument gripper_center is set to `true`. This creates a new frame at the tip of the robot end effector that can be used while doing motion planning with MoveIt! (instead of using panda_link8 which is offset from the actual gripper joints)

---

TODO:

1. Make it easy to move back and forth from simulation to the real robot. I think it mainly needs changes to the controller list in the MoveIt! config.