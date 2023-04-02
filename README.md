# Aruco-Detection-Using-IRIS-Drone---Ardupilot-SITL-in-Gazebo by Drone_Singh

All the instructions are present in the Commands.docx with all instructions and required dependencies to install.

Applications and Dependencies required to run the simulation
Installing Ardupilot

Clone ArduPilot
cd ~
sudo apt install git
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot

Install dependencies:
cd ardupilot
Tools/environment_install/install-prereqs-ubuntu.sh -y


reload profile
. ~/.profile

Checkout Latest Copter Build
git checkout Copter-4.3.4
git submodule update --init –recursive

Run SITL (Software In The Loop) once to set params:
cd ~/ardupilot/ArduCopter
sim_vehicle.py -w


Installing Gazebo

sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

Setup keys:
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

Reload software list:
sudo apt update
According to your OS
Ubuntu [18.04]
sudo apt install gazebo9 libgazebo9-dev

Ubuntu [20.04]
sudo apt-get install gazebo11 libgazebo11-dev


Install Gazebo plugin for APM (ArduPilot Master) :

cd ~
git clone https://github.com/khancyr/ardupilot_gazebo.git
cd ardupilot_gazebo

For only Ubuntu 18.04 OS only
git checkout dev  (This command not required in Ubuntu 20.04 OS)

Comman commands for both OS -
mkdir build
cd build
cmake ..
make -j4
sudo make install

echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc

Set paths for models:
echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc
. ~/.bashrc

Download QGroundControl

Run Simulator
In one Terminal (Terminal 1), run Gazebo:
gazebo --verbose ~/ardupilot_gazebo/worlds/iris_arducopter_runway.world

In another Terminal (Terminal 2), run SITL:
cd ~/ardupilot/ArduCopter/
../Tools/autotest/sim_vehicle.py -f gazebo-iris --console –map
If everything is opening and working without error, all installations are correct.




Make the following changes -
1. In your Home directory of the computer go to .gazebo file
/home/(your user name)/.gazebo/models >> go and paste the aruco_visual_marker_0 file in the models folder.
2. Paste the aruco.world file in the worlds folder of ardupilot_gazebo folder -
/home/(your user name)/ardupilot_gazebo/worlds
3. Put the MultiMatrix.npz file in Downloads folder of your PC and don’t extract it. Put its path in the night.py file in the variable calib_data_path.
calib_data_path = "/home/(your user name)/Downloads/MultiMatrix.npz"
Python Script to Autonomously land drone aruco marker
Download the python script – night.py (It contains all the code to automate the drone for aruco marker detection)

How to see the final simulation
Step 0 -
Run roscore command 

Step 1 - 
In one Terminal (Terminal 1), run Gazebo:
gazebo --verbose ~/ardupilot_gazebo/worlds/aruco.world

Step 2 -
In another Terminal (Terminal 2), run SITL:
cd ~/ardupilot/ArduCopter/
../Tools/autotest/sim_vehicle.py -f gazebo-iris --console –map

Step 3 -
Run QGroundControl App for reference purposes.
On opening once it gets connected and shows Ready To Fly on top left.
Make sure to keep the mode in Guided Mode.







Step 4 -
In another Terminal (Terminal 3), run the python script:
Download the python script – night.py (It contains all the code to automate the drone for aruco marker detection)
My script was in Downloads folder
cd Downloads
python3 night.py

Now view the simulation in Gazebo.

Thank You
Best Regards
Drone_Singh
