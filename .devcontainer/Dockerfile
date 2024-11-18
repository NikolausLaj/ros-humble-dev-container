FROM osrf/ros:humble-desktop-full

# Add ubuntu user with same UID and GID as your host system, if it doesn't already exist
# Since Ubuntu 24.04, a non-root user is created by default with the name vscode and UID=1000
ARG USERNAME=ubuntu
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN if ! id -u $USER_UID >/dev/null 2>&1; then \
        groupadd --gid $USER_GID $USERNAME && \
        useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME; \
    fi
# Add sudo support for the non-root user
RUN apt-get update && \
    apt-get install -y sudo && \
    echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Switch from root to user
USER $USERNAME

# Add user to video group to allow access to webcam
RUN sudo usermod --append --groups video $USERNAME

# Update all packages
RUN sudo apt update && sudo apt upgrade -y

# Install Git
RUN sudo apt install -y git

# Install wget
RUN sudo apt install -y wget

# Rosdep update
RUN rosdep update

# libs for Python ROS2 nodes
RUN sudo apt-get install -y \
    python3-colcon-common-extensions \
    python3-pip \
    python3-setuptools \
    python3-rosdep \
    python3-serial

# OpenCV
RUN pip install \
    opencv-python \
    opencv-python-headless

# Additional ROS2 pakages
RUN sudo apt-get install \
    ros-humble-v4l2-camera

# For Serial Connection
RUN sudo usermod -aG dialout $USERNAME

# Source the ROS setup file
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc
