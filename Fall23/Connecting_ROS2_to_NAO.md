# ROS2 on Dell Laptop
ROS2 and the naoqi_driver2 are fully functional on the Dell laptop. These instructions are for starting ROS2 up and connecting to the NAO on that machine.

## Dell Laptop
- The laptop password is the same as the username: robot-boss
- *The keyboard is really junk and you have to watch what is actually typed!!*
- Press the F12 key to drop down the terminal (this terminal app is called Yakuake)

- make sure the terminal is in `~/ros2_humble` directory.
  ```
  cd ~/ros2_humble
  ```
- You will need probably 3 terminal windows/Shells. Press the `+` on the left side of the terminal lower bar to open more. Yakuake automatically opens new windows in the same directory.

## NAO robot
 - Start up one of the NAOs
 - Once fully on, press the chest button to get the robot to tell you it's IP number/"Internet address"
 - Make note of robot password (on tape on back of robot)
   
### Turn off NAO autonomous life via SSH
 - In the terminal on the computer, connect to NAO
   ```
   ssh nao@ROBOTS_IP_ADDRESS
   ```
   - Where ROBOTS_IP_ADDRESS is the one you got from the NAO.
  - You be asked to to enter the robot's password
  - The command prompt will change when SSH connection to the NAO is established.
  - At that prompt enter:
    ```
    qicli call ALAutonomousLife.setState disabled
    ```
    - It will take a second or to and the robot will sigh and kneel down if it isn't already.
    - When the command prompt comes back in the terminal, enter:
      ```
      qicli call ALMotion.wakeUp
      ```
      - The robot will stand up.
    - Leave SSH session in terminal:
      ```
        exit
      ```
### Launch naoqi_driver in ROS2
  - In the terminal (after SSH session is exited)
    ```
    ros2 launch naoqi_driver naoqi_driver.launch.py nao_ip:=ROBOTS_IP_ADDRESS
    ```
     -  Where ROBOTS_IP_ADDRESS is again the one you got from the NAO.
- In a second terminal window:
  ```
  ros2 node info /naoqi_driver
  ```
  - This will print out all the information about the driver if it is running
 
  - After this, the robot is ready to work with.
 
## Examples
### Hello, world 

Make the robot say hello. In another terminal:
```
ros2 topic pub --once /speech std_msgs/String "data: hello"
```

### Listen to words

You can setup speech recognition and get one result.
```
ros2 action send_goal listen naoqi_bridge_msgs/action/Listen "expected: [\"hello\"]
language: \"en\""
```

### Move the head

Check that you can move the head by publishing on /joint_angles:
```
ros2 topic pub --once /joint_angles naoqi_bridge_msgs/JointAnglesWithSpeed "{header: {stamp: now, frame_id: ''}, joint_names: ['HeadYaw', 'HeadPitch'], joint_angles: [0.5,0.1], speed: 0.1, relative: 0}"
```

You can see the published message (in another terminal window) with:
```
ros2 topic echo /joint_angles
```

### Move around

- Make some room around the robot!

- Check that you can make the robot walk in a circle by publishing on cmd_vel to make the robot move:
```
ros2 topic pub --once /cmd_vel geometry_msgs/Twist "linear:
  x: 0.5
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.785"
```

- To stop the robot, you must send a new message with linear and angular velocities set to 0:
```
ros2 topic pub --once /cmd_vel geometry_msgs/Twist "linear:
  x: 0.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0"
```

### Info
 - The (out of date/for ROS1) documentation for the naoqi_driver is built on the Dell. You can read it in the browser. It is bookmarked in Firefox and is on the bookmarks toolbar (the two rightmost tabs)
 - Warning - documentation is hard to decipher and is for ROS1. I suggest practicing with ROS2 tutorials (for other robots) before trying to use it/translate from ROS1 to ROS2 commands.
