FROM ghcr.io/soham2560/noetic:latest

# Add the following labels
LABEL org.opencontainers.image.description="LioSAM"
LABEL org.opencontainers.image.title="LioSAM"
LABEL org.opencontainers.image.vendor="Nishant"
LABEL org.opencontainers.image.source="https://github.com/NishantChandna1403/Lio-SAM-Docker_Noetic.git"
LABEL maintainer="chandna.nishant@gmail.com"
LABEL org.opencontainers.image.licenses="MIT"
# handle default shell
SHELL ["/bin/bash", "-c"]

# Install Basic deps
RUN sudo apt-get update \
    && sudo apt-get -y install --no-install-recommends\
    libssl-dev \
    libusb-1.0-0-dev \
    libudev-dev \
    pkg-config \
    libgtk-3-dev \
    wget \
    cmake \
    build-essential \
    libglfw3-dev \
    libgl1-mesa-dev \
    libglu1-mesa-dev \
    at \
    libaria-dev \
    v4l-utils \
    xterm \
    && sudo apt-get autoremove -y \
    && sudo apt-get clean -y \
    && sudo rm -rf /var/lib/apt/lists/*

# Build and install Realsense-SDK
WORKDIR /home/container_user
RUN git clone --branch v2.56.2 https://github.com/IntelRealSense/librealsense.git
WORKDIR /home/container_user/librealsense
RUN sudo mkdir -p /etc/udev/rules.d && \
    ./scripts/setup_udev_rules.sh \
    ./scripts/patch-realsense-ubuntu-lts-hwe.sh 
RUN mkdir build && cd build \
    && cmake ../ \
    && sudo make uninstall && make clean && make && sudo make install 

COPY rosPkgs.list /tmp/rosPkgs.list
RUN sudo apt-get update \
    && sudo apt-get -y install --no-install-recommends $(cat /tmp/rosPkgs.list) \
    && sudo rm -rf /var/lib/apt/lists/*
WORKDIR /home/container_user
RUN git clone https://github.com/Livox-SDK/Livox-SDK.git
WORKDIR /home/container_user/Livox-SDK
RUN mkdir -p build && cd build \
&& cmake .. \
&& make \
&& sudo make install

WORKDIR /home/container_user


    
