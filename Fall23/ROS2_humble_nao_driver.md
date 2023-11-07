# Working Setup:
 - Ubuntu 22.04 LTS
 - ROS2 Humble
 - naoqi_driver2 (fix_humble branch)

## Installation
### 1. Install Ubuntu 22.04 LTS
 - https://ubuntu.com/download/desktop

### 2. Install ROS2 Humble
  - You HAVE to install from the source, not using Ubuntu `apt install`
  - The ROS2 source is on GitHub.
  - Instructions below are modified from this site: https://docs.ros.org/en/humble/Installation/Alternatives/Ubuntu-Development-Setup.html#system-setup
  

#### 2.a. Set locale

Make sure you have a locale which supports UTF-8. If you are in a minimal environment (such as a docker container), the locale may be something minimal like POSIX. We test with the following settings. However, it should be fine if you’re using a different UTF-8 supported locale.
```
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```
#### 2.b Add the ROS 2 apt repository

You will need to add the ROS 2 apt repository to your system.

First ensure that the Ubuntu Universe repository is enabled.
```
sudo apt install software-properties-common
sudo add-apt-repository universe
```
Now add the ROS 2 GPG key with apt.

```
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

Then add the repository to your sources list.
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

#### 2.c. Install development tools and ROS tools

Install common packages.
```
sudo apt update && sudo apt install -y \
  python3-flake8-docstrings \
  python3-pip \
  python3-pytest-cov \
  ros-dev-tools
```
Install python packages for Ubuntu 22.04.
```
sudo apt install -y \
   python3-flake8-blind-except \
   python3-flake8-builtins \
   python3-flake8-class-newline \
   python3-flake8-comprehensions \
   python3-flake8-deprecated \
   python3-flake8-import-order \
   python3-flake8-quotes \
   python3-pytest-repeat \
   python3-pytest-rerunfailures
```
#### 2.d. Get ROS 2 code

Create a workspace (folder in your home directory in Ubuntu) and clone ROS2 repo from GitHub:
```
mkdir -p ~/ros2_humble/src
cd ~/ros2_humble
vcs import --input https://raw.githubusercontent.com/ros2/ros2/humble/ros2.repos src
```
The ROS2 repo will now be in your `ros2_humble/src/` directory

You can double check that it is on the humble branch!
```
cd src/ros2
git checkout humble
```

#### 2.e Install dependencies using rosdep

ROS 2 packages are built on frequently updated Ubuntu systems. It is always recommended that you ensure your system is up to date before installing new packages.
```
sudo apt upgrade

sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```
#### 2.f. Build the ROS2 code in the workspace

- If you have already installed ROS 2 another way (either via Debians or the binary distribution), make sure that you run the below commands in a fresh environment that does not have those other installations sourced. Also ensure that you do not have source /opt/ros/${ROS_DISTRO}/setup.bash in your .bashrc. You can make sure that ROS 2 is not sourced with the command printenv | grep -i ROS. The output should be empty.

```
cd ~/ros2_humble/
colcon build --symlink-install
```
This will cause a lot of stuff to happen and may take a while to complete!

ROS2 should be successfully built and ready to use at this point.

#### 2.g Test ROS2 Install

##### Environment setup
Source the setup script. **This has to be done for each terminal window you open to use ROS**

Set up your environment by sourcing the following file. *Note there is a dot at the beginning of the line:*
```
. ~/ros2_humble/install/local_setup.bash
```
##### Try some examples to make sure ROS2 is working

In one terminal, source the setup file and then run a C++ talker:
```
. ~/ros2_humble/install/local_setup.bash
ros2 run demo_nodes_cpp talker
```
In another terminal source the setup file and then run a Python listener:
```
. ~/ros2_humble/install/local_setup.bash
ros2 run demo_nodes_py listener
```
You should see the talker saying that it’s Publishing messages and the listener saying I heard those messages. This verifies both the C++ and Python APIs are working properly. Hooray!

### 3. Install naoqi_driver2
This is the tricky part!!!
- Instructions modified from here: https://github.com/ros-naoqi/naoqi_driver2/tree/fix_humble
- Contrary to what the instructions say, you cannot install the other naoqi dependencies using apt get. You have to use vcs import as below.
#### 3.a. Clone naoqi_driver2 repository
Starting in your ~/ros2_humble directory:
```
  cd src/
  git clone https://github.com/ros-naoqi/naoqi_driver2.git
  ## go into the repo and checkout the fix_humble branch or nothing will work
  cd naoqi_driver2/
  git checkout fix_humble
  cd ..
  vcs import < naoqi_driver2/dependencies.repos
```
At this point the instructions assume the thing is ready to build, but NO! There are some other libraries and ROS2 packages needed or building will fail.

#### 3.b. Install numpy and libboost:
```
  sudo apt install python3-numpy
  sudo apt install libboost-python-dev
```
#### 3.c. Clone ROS2 OpenCV vision package to src folder
```
  cd ~/ros2_humble/src/
  git clone https://github.com/ros-perception/vision_opencv.git -b ros2
```
#### 3.d. Clone ROS2 diagnostics package
Also to src folder:

```
  git clone https://github.com/ros/diagnostics.git -b ros2
```
#### 3.e. NOW attempt to build packages
```
  cd ~/ros2_humble
  colcon build --symlink-install
```
This will build all packages and will take a while!! On the Dell it took a least an hour the first time.
- Watch for two installer popups that will come up about the Pepper and Nao meshes packages. You have to accept some terms and click a few buttons or build will not finish.

  - You need to watch for errors. When build finishes it will say if any package builds failed. I had to run it at least twice to get everything to build correctly.
      - It gets faster as more packages are already successfully built.
  - You can also try to build a single package (if one fails) using this command:
  - `colcon build --packages-select <name-of-pkg>`
  - The build error logs are in ~/ros2_humble/logs folder. I had to go through these to find missing packages.
 
#### 3.f. Test with NAO
  - After colcon build is successful (no failures)
  - Follow the instructions on the naoqi_driver page: https://github.com/ros-naoqi/naoqi_driver2/tree/fix_humble#launch
  - You must always enter
  - `source ~/ros2_humble/install/setup.bash` when you open a new terminal
