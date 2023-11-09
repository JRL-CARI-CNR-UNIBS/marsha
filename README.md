# marsha

## Build/Installation
The software can be installed using rosinstall files.

1. Install ROS: follow the steps described in http://wiki.ros.org/ROS/Installation
2. Install Git: follow the steps described in https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
3. Install wstool: follow the steps described in http://wiki.ros.org/wstool
4. Install rosdep: follow the steps described in http://wiki.ros.org/rosdep
5. Install catkin_tools: follow the steps described in https://catkin-tools.readthedocs.io/en/latest/installing.html

Create your workspace:
```
mkdir -p ~/marsha_ws/src
cd ~/marsha_ws
catkin init
wstool init src
```
The main dependency of MARSHA is [OpenMORE](https://github.com/JRL-CARI-CNR-UNIBS/OpenMORE.git). You can use its rosinstall file to dowload the required dependencies:
```
cd ~/marsha_ws
wget https://raw.githubusercontent.com/JRL-CARI-CNR-UNIBS/OpenMORE/master/OpenMORE.rosinstall
wstool merge -t src ./OpenMORE.rosinstall
wstool update -t src
rosdep install --from-paths src --ignore-src -r -y
```
Download the other dependencies for MARSHA:
```
cd ~/marsha_ws/src
git clone https://github.com/JRL-CARI-CNR-UNIBS/human_aware_cost_functions.git
git clone --recurse-submodules https://github.com/JRL-CARI-CNR-UNIBS/thread-pool.git
```
Now, compile the workspace:
```
cd ~/marsha_ws/src
catkin build -cs
```
And source the workspace:
```
echo "source /home/$USER/marsha_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### Docker
A [docker file](https://github.com/JRL-CARI-CNR-UNIBS/marsha/blob/master/dockerfile_marsha) is also available. Open a terminal, move into the folder where you have saved the docker file and run the following command:
```
sudo docker build -f dockerfile_marsha -t marsha .
```
Once completed, run the container:
```
xhost + 

sudo docker run -it --net=host --gpus all \
    --env="NVIDIA_DRIVER_CAPABILITIES=all" \
    --env="DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    marsha
```
Then, inside the container you can use the algorithm.
