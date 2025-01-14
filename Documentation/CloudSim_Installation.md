# Prerequisites:

* ubuntu 18.04
* docker
* kubernetes
* agones
* mongodb
* ROS
* swipl
* knowrob
* knowrob_ameva



## Docker

* from https://docs.docker.com/engine/install/ubuntu/


  * `$ sudo apt-get update && sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release`
  * `$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
  * `$ echo \
    "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
  * `$ sudo apt-get update && sudo apt-get install docker-ce docker-ce-cli containerd.io` 

## Nvidia Support

- from https://phoenixnap.com/kb/install-nvidia-drivers-ubuntu

  - ubuntu-drivers devices (Get supported driver)
  - sudo apt install nvidia-driver-450 (Tested version)
  - nvidia-smi (make sure you installed the driver, after reboot)

- https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker

  - distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
       && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
       && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
  - sudo apt-get update
  - sudo apt-get install -y nvidia-docker2
  - sudo systemctl restart docker
  - sudo docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi （make sure you can use nvidia driver in container）

- https://levelup.gitconnected.com/running-gpu-enabled-containers-in-kubernetes-cluster-f0a3d87a450c

  - Edit docker runtime at /etc/docker/daemon.json

    ```
    {
        "default-runtime": "nvidia",
        "runtimes": {
            "nvidia": {
                "path": "/usr/bin/nvidia-container-runtime",
                "runtimeArgs": []
            }
        }
    }
    ```

  - sudo pkill -SIGHUP dockerd

  - sudo systemctl daemon-reload

  - sudo systemctl restart docker

  - sudo docker run --rm  nvidia/cuda:11.0-base nvidia-smi (make sure the default runtime is nvidia)

## Kubernetes

### kubeadm, kubelet and kubectl

* from https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

  * `$ sudo apt-get update`
  * `$ sudo apt-get install -y apt-transport-https ca-certificates curl`
  * `$ sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg`
  * `$ echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list`
  * `$ sudo apt-get update`
  * `$ sudo apt-get install -y kubelet=1.16.15-00 kubeadm=1.16.15-00 kubectl=1.19.2-00`
  * `$ sudo apt-mark hold kubelet kubeadm kubectl`




## MongoDB

* from https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/:

### libmongoc:

* from http://mongoc.org/libmongoc/current/installing.html

  * `$ sudo apt-get install libmongoc-1.0-0`



## ROS

* from https://wiki.ros.org/melodic/Installation/Ubuntu

 * `$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`
 * `$ sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654`
 * `$ sudo apt update && sudo apt install ros-melodic-desktop`
 * `$ echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc`
 * `$ source ~/.bashrc`



## Prolog

* from https://www.swi-prolog.org/build/PPA.html:

  * `$ sudo apt-add-repository ppa:swi-prolog/stable`
  * `$ sudo apt-get update && sudo apt-get install swi-prolog`

  

## KnowRob

* from https://github.com/knowrob/knowrob:

  * `$ sudo apt install python-rosdep2 python-wstool rosbash`
  * `$ cd ~ && mkdir catkin_ws && cd catkin_ws && rosdep update`
  * `$ wstool init src`
  * `$ cd src`
  * `$ wstool merge https://raw.github.com/knowrob/knowrob/master/rosinstall/knowrob-base.rosinstall`
  * `$ wstool update`
  * `$ rosdep install --ignore-src --from-paths .`
  * `$ cd ~/catkin_ws && catkin_make`
  * `$ echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc`  
  * `$ echo "export SWI_HOME_DIR=/usr/lib/swi-prolog" >> ~/.bashrc`  
  * `$ source ~/.bashrc`


### knowrob_ameva

* websockets:

  * `$ sudo apt-get install libwebsockets-dev`

* protobuf (https://github.com/protocolbuffers/protobuf/blob/master/src/README.md):

  * `$ sudo apt-get install autoconf automake libtool curl make g++ unzip`
  * `$ cd ~ && mkdir thirdparty && cd thirdparty`
  * `$ git clone https://github.com/protocolbuffers/protobuf.git`
  * `$ cd protobuf`
  * `$ git submodule update --init --recursive`
  * `$ ./autogen.sh`
  * `$ ./configure`
  * `$ make`
  * `$ make check`
  * `$ sudo make install`
  * `$ sudo ldconfig`


* knowrob_ameva:

  * `$ sudo apt install libcurlpp-dev`
  * `$ cd ~/catkin_ws/src`
  * `$ git clone https://github.com/robcog-iai/knowrob_ameva.git`
  * `$ cd ~/catkin_ws && catkin_make knowrob_ameva`

