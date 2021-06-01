### Package CloudSim Level in a Docker Image in Linux

Notice: Compiling in Linux is more strict than in Windows. Some error will not occur in Windows. To compile the project we need to use the tool `ue4-docker` to build Unreal Engine in Linux and use the image to compile the project

* Install ue4-docker. https://pypi.org/project/ue4-docker/

  ```
  pip install ue4-docker
  ```

* Build container images for the Unreal Engine 4.23 version Pixel Streaming for Linux. https://docs.adamrehn.com/ue4-docker/commands/build

  ```
  ue4-docker build \
    custom:4.23.1-pixelstreaming \                        # Tag the image as adamrehn/ue4-full:4.23.1-pixelstreaming
    -repo=https://github.com/adamrehn/UnrealEngine.git \  # Use Adam's fork of the Unreal Engine
    -branch=4.23.1-pixelstreaming \                       # Use the branch for the Engine version we are targeting
    --no-engine                                           # Don't build the ue4-engine image, just source, minimal and full
  ```

* Use Dockerfile to build the image with RobCoG project. The project needs to be built with Linux with NVIDIA driver.

  ```
  docker build -t robcog/map_name .
  ```

* Upload the image to docker images repo. The cloudsim_k8s_launcher will pull the image from docker hub, therefore you need to push the image to docker hub.

  ```
  docker push robcog/map_name
  ```

  
