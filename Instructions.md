# Pioneer 3 AT configuration in 2019 (with Raspberry Pi)
## Hardware
- Pioneer 3-AT
- Microsoft Kinect
- Raspberry Pi 3, Model B
## Software
- Ubuntu 18.04
- Ubuntu Mate 18.04
- ROS Melodic Morenia
- RosAria
- ARIA
- Openni
- libfreenect (freenect)

## How to
### Basic installation (PC or laptop)
1. Install Ubuntu Bionic (18.04.):
https://ubuntu.com/#download
2. Install ROS, full installation recommended:
http://wiki.ros.org/melodic/Installation/Ubuntu
3. Configure your ROS workspace:
http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment
4. Install RosAria, use instructions for ROS Melodic:
http://wiki.ros.org/ROSARIA/Tutorials/How%20to%20use%20ROSARIA
5. Install and enable use of the Kinetic:

*sudo apt-get install ros-melodic-openni-camera*

### Installations of Raspberry Pi
1. Download and install the recommended version for the RPi OS Ubuntu Mate 18.04., ARMv7:
https://ubuntu-mate.org/download/
2. Connect your SD card on a laptop or PC with linux, run the following command to install Ubuntu Mate on the SD card: 

*sudo dd if=ubuntu-mate-18.04.2-beta1-desktop-armhf+raspi-ext4.img
of=/dev/mmcblk0 status=progress* 

mmcblk0 is the SD drive, recommended to check if it's the same on your system using *lsblk*
Or you can use https://www.balena.io/etcher/ - Windows also.
Put the SD into RPi and the standard Linux installation will begin.
3. Enable software installation from other sources (universe, retricted and multiverse) uncommenting lines in */etc/apt/sources.list* or in your Linux settings under Software&Updates.
4. Standard update:

*sudo apt-get update & upgrade*

5. To use SSH you have to reinstall openssh (not sure if a bug, but it didn't work otherwise)

*sudo apt-get remove openssh-server*

*sudo apt-get install openssh-server*

Afterwards restart the device and enable ssh:

*sudo systemctl enable ssh*

6. Repeat installation steps from previous section (2-5)
7. ARIA installation (if it doesn't work with the official instructions):

*make* in ARIA source code root directory

*make* in ArNetworking subdirectory

*sudo make install* from root directory to install

7. The next commands will install libfreenect if you are using Microsoft Kinect:

*git clone https://github.com/OpenKinect/libfreenect.git*

*cd libfreenect*

*mkdir build*

*cd build*

*cmake -L ..*

*make*

*sudo make install*

*cd ~/catkin_workspace/src*

*git clone https://github.com/ros-drivers/freenect_stack.git*

*cd ..*

*catkin_make*

Installation is done, if you are not using a RPi, you can install everything on the laptop only, but connecting a RPi remotely is more practical.
### Configuring laptop and RPi communication
On your laptop: 

*export ROS_MASTER_URI=http://name-ros.local:11311*

*export ROS_IP=192.168.43.210*

MASTER_URI is your local server name on your RPi and it's port and ROS_IP is the IP address of the client.

On your RPi:

*export ROBOT_NAME=name-ros*

*export ROS_HOSTNAME=name-ros.local*

*export ROS_MASTER_URI=http://$ROS_HOSTNAME:11311*
### Running
Assuming you have your own launch files and configurations, don't forget to give these permissions to the USB ports where your Pioneer is connected:

*sudo chmod 775 dev/ttyUSB0*

s*udo chown kusername dev/ttyUSB0*
