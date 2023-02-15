- [About](#org95f4cc4)
- [Usage](#orgd9d6b1c)
- [Topics](#orgcb6ba48)
- [Setup](#org1fcf85c)
- [Development](#org4dcbb7e)

    <!-- This file is generated automatically from metadata -->
    <!-- File edits may be overwritten! -->


<a id="org95f4cc4"></a>

# About

```markdown
- ROS Package Names:
  - weigher
  - weigher_interfaces
- ROS Distribution: humble
- Description: This repository contains ROS 2 packages that publish weight messages from a digital scale.
- Version: 1.0.0
- Weigher Package: weigher
- Weigher Launch File: weigher_launch.py
- Weigher Topics:
  - /weight
  - /weight_thresholded
- Weigher Messages:
  - Weight.msg
- Release Date: 2023-02-15
- Creation Date: 2022-12-14
- License: BSD-3-Clause
- URL: https://github.com/janelia-ros/weigher_ros
- Author: Peter Polidoro
- Email: peter@polidoro.io
- Copyright: 2023 Howard Hughes Medical Institute
- References:
  - https://github.com/janelia-pypi/loadstar_sensors_interface_python
- Python Dependency List: loadstar_sensors_interface
```


<a id="orgd9d6b1c"></a>

# Usage


## Publisher

-   Connect weigh scale to Raspberry Pi USB port.
-   Connect Raspberry Pi to power and Ethernet.

Scale will be tared on power up and automatically begin publishing weight messages.


## Subscriber

On a computer connected to the same network as the Raspberry Pi, either use Docker or install ROS 2 to subscribe to weight messages.


### Echo weight topics on network computer

1.  Docker

    ```sh
    docker run -it --net=host --pid=host weigher:latest ros2 topic echo /weight
    docker run -it --net=host --pid=host weigher:latest ros2 topic echo /weight_thresholded
    ```

2.  ROS 2 on Ubuntu

    ```sh
    source ~/ros2_ws/install/setup.bash
    ros2 topic echo /weight
    ros2 topic echo /weight_thresholded
    ```


## Connect to Raspberry Pi for development and monitoring


### SSH into Raspberry Pi

```sh
ssh weigher@weigher.local
```


### Web Console

<https://weigher.lan:9090/>

-   username: weigher
-   password: ******\*******


### Echo weight topic in Raspberry Pi terminal

```sh
echo-weight
```


<a id="orgcb6ba48"></a>

# Topics


## weight

    ---
    stamp:
      sec: 1676470173
      nanosec: 765260627
    weight: 0.13607771100000002
    ---
    stamp:
      sec: 1676470173
      nanosec: 883177140
    weight: 0.0
    ---
    stamp:
      sec: 1676470174
      nanosec: 11527425
    weight: 0.04535923700000001
    ---
    stamp:
      sec: 1676470174
      nanosec: 176475007
    weight: 0.18143694800000004
    ---

```text
---
stamp:
  sec: 1676470173
  nanosec: 765260627
weight: 0.13607771100000002
---
stamp:
  sec: 1676470173
  nanosec: 883177140
weight: 0.0
---
stamp:
  sec: 1676470174
  nanosec: 11527425
weight: 0.04535923700000001
---
stamp:
  sec: 1676470174
  nanosec: 176475007
weight: 0.18143694800000004
---
```


## weight\_thresholded

    ---
    stamp:
      sec: 1676470255
      nanosec: 932870887
    weight: 520.8601184710001
    ---
    stamp:
      sec: 1676470256
      nanosec: 19947998
    weight: 504.39471544000014
    ---
    stamp:
      sec: 1676470256
      nanosec: 161346684
    weight: 499.8134325030001
    ---
    stamp:
      sec: 1676470256
      nanosec: 301352968
    weight: 498.5887331040001
    ---

```text
---
stamp:
  sec: 1676470255
  nanosec: 932870887
weight: 520.8601184710001
---
stamp:
  sec: 1676470256
  nanosec: 19947998
weight: 504.39471544000014
---
stamp:
  sec: 1676470256
  nanosec: 161346684
weight: 499.8134325030001
---
stamp:
  sec: 1676470256
  nanosec: 301352968
weight: 498.5887331040001
---
```


<a id="org1fcf85c"></a>

# Setup


## Subscriber


### Docker

1.  Install Docker

    <https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository>

2.  Clone this repository

    ```sh
    git clone git@github.com:janelia-ros/weigher_ros.git
    ```

3.  Make Docker image

    ```sh
    cd weigher_ros
    docker build -f .metadata/docker/Dockerfile -t weigher:latest .
    # or
    make -f .metadata/Makefile docker-image
    ```


### ROS 2 on Ubuntu

1.  Install ROS 2

    ```text
    https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html
    ```

2.  Create ROS 2 Workspace and clone this repository

    ```sh
    mkdir -p ~/ros2_ws/src && \
    cd ~/ros2_ws/src && \
    git clone git@github.com:janelia-ros/weigher_ros.git
    ```

3.  Setup Python virtualenv

    ```sh
    sudo apt install python3-venv
    cd ~/ros2_ws
    make -f src/weigher_ros/.metadata/Makefile virtualenv
    ```

4.  Build ROS packages

    1.  Source the ROS underlay and activate the Python virtualenv and build ROS packages
    
        ```sh
        # build may finish with stderr warnings about deprecated setup.py install
        # if using Python 3.10 or higher
        cd ~/ros2_ws && \
        make -f src/weigher_ros/.metadata/Makefile ros-build
        ```


## Publisher


### Raspberry Pi

1.  Setup Raspberry Pi

    <https://github.com/janelia-experimental-technology/raspberrypi_setup>
    
    -   username: weigher
    -   hostname: weigher

2.  SSH into Raspberry Pi

    ```sh
    ssh weigher@weigher.local
    ```

3.  Web Console

    <https://weigher.lan:9090/>

4.  Clone Repository

    ```sh
    cd ~ && \
    git clone git@github.com:janelia-ros/weigher_ros.git
    ```

5.  Add deploy ssh key to Github Repository

    ```sh
    cat .ssh/id_ed25519.pub
    ```

6.  Install make for metadata commands

    ```sh
    sudo apt install make
    ```

7.  Docker image

    ```sh
    cd ~/weigher_ros && \
    make -f .metadata/Makefile docker-image
    ```

8.  Host Setup

    ```sh
    cd ~/weigher_ros && \
    make -f .metadata/Makefile host-setup
    sudo reboot
    ```

9.  Check docker and systemd service

    ```sh
    systemctl status docker
    systemctl status weigher-attached@00.service
    systemd-analyze plot > boot_analysis.svg
    ```


<a id="org4dcbb7e"></a>

# Development


## Docker


### Run Docker container

```sh
make -f .metadata/Makefile docker-container
```


### Run Docker container with serial port access

```sh
make -f .metadata/Makefile docker-container-port
```


### Run Docker container and start publishing weight messages

```sh
make -f .metadata/Makefile docker-publish-weight
```


### Run Docker container and echo weight messages

```sh
make -f .metadata/Makefile docker-echo-weight
```


### Stop all Docker containers

```sh
docker stop $(docker ps -aq)
```


### Find running container Name

```sh
docker ps
```


### Run bash commands in running container

```sh
docker exec -it container-name bash
```


## Ubuntu


### Build

1.  Source the ROS underlay and activate the Python virtualenv and build ROS packages

    ```sh
    # build may finish with stderr warnings about deprecated setup.py install
    # if using Python 3.10 or higher
    cd ~/ros2_ws && \
    make -f src/weigher_ros/.metadata/Makefile ros-build
    ```


### Run

1.  Source the ROS underlay and overlay and activate Python virtualenv and run the weigher node

    1.  Launch file
    
        ```sh
        source ~/ros2_ws/src/weigher_ros/.metadata/setup.bash && \
        source ~/ros2_ws/install/setup.bash && \
        ros2 launch weigher weigher_launch.py
        ```
    
    2.  ROS Run
    
        ```sh
        source ~/ros2_ws/src/weigher_ros/.metadata/setup.bash && \
        source ~/ros2_ws/install/setup.bash && \
        ros2 run weigher weigher
        ```

2.  Echo the weigher topic

    ```sh
    # Open a new termial
    source ~/ros2_ws/install/setup.bash
    ros2 topic echo /weight
    ros2 topic echo /weight_thresholded
    ```


## Raspberry Pi


### Update

```sh
cd ~/weigher_ros
git pull
make -f .metadata/Makefile docker-image
make -f .metadata/Makefile host-setup
sudo reboot
```


## Metadata


### Install Guix

[Install Guix](https://guix.gnu.org/manual/en/html_node/Binary-Installation.html)


### Clone Repository

```sh
git clone git@github.com:janelia-ros/weigher_ros.git
```


### Edit metadata.org

```sh
make -f .metadata/Makefile metadata-edits
```


### Tangle metadata.org

```sh
make -f .metadata/Makefile metadata
```