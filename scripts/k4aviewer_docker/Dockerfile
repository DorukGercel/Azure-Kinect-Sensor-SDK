FROM ubuntu:18.04
RUN apt update
RUN apt install -y sudo
RUN sudo apt install -y git
RUN sudo apt install -y wget
RUN sudo apt install -y build-essential
RUN sudo apt install libssl-dev
RUN rm -rf /var/lib/apt/lists/*
RUN cd /tmp && wget https://github.com/Kitware/CMake/releases/download/v3.20.0/cmake-3.20.0.tar.gz && tar -zxvf cmake-3.20.0.tar.gz && cd cmake-3.20.0 && ./bootstrap
WORKDIR /tmp/cmake-3.20.0
RUN make
RUN sudo make install
WORKDIR /
RUN git clone https://github.com/microsoft/Azure-Kinect-Sensor-SDK.git && mkdir Azure-Kinect-Sensor-SDK/build
RUN sudo apt update
RUN sudo apt install -y ninja-build
RUN sudo apt install -y libsoundio-dev && sudo apt-get install -y libusb-1.0-0-dev
RUN sudo apt install -y libx11-dev
RUN sudo apt install -y xorg-dev libglu1-mesa-dev
RUN sudo apt install -y uuid-dev
RUN sudo apt install -y libudev-dev
WORKDIR /Azure-Kinect-Sensor-SDK/build
RUN cmake .. -GNinja
RUN ninja
COPY ../99-k4a.rules /etc/udev/rules.d/
RUN sudo apt update
RUN sudo apt install -y curl
RUN sudo apt install -y software-properties-common
RUN curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
RUN sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
RUN sudo apt update
RUN curl -sSL https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/libk/libk4a1.3/libk4a1.3_1.3.0_amd64.deb > /tmp/libk4a1.3_1.3.0_amd64.deb
RUN curl -sSL https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/libk/libk4a1.3-dev/libk4a1.3-dev_1.3.0_amd64.deb > /tmp/libk4a1.3-dev_1.3.0_amd64.deb
RUN curl -sSL https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/libk/libk4abt1.0/libk4abt1.0_1.0.0_amd64.deb > /tmp/libk4abt1.0_1.0.0_amd64.deb
RUN curl -sSL https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/libk/libk4abt1.0-dev/libk4abt1.0-dev_1.0.0_amd64.deb > /tmp/libk4abt1.0-dev_1.0.0_amd64.deb
RUN curl -sSL https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/k/k4a-tools/k4a-tools_1.3.0_amd64.deb > /tmp/k4a-tools_1.3.0_amd64.deb
RUN echo 'libk4a1.3 libk4a1.3/accepted-eula-hash string 0f5d5c5de396e4fee4c0753a21fee0c1ed726cf0316204edda484f08cb266d76' | debconf-set-selections
RUN echo 'libk4abt1.0 libk4abt1.0/accepted-eula-hash string	03a13b63730639eeb6626d24fd45cf25131ee8e8e0df3f1b63f552269b176e38' | debconf-set-selections
RUN dpkg -i /tmp/libk4a1.3_1.3.0_amd64.deb
RUN dpkg -i /tmp/libk4a1.3-dev_1.3.0_amd64.deb
RUN dpkg -i /tmp/libk4abt1.0_1.0.0_amd64.deb
RUN dpkg -i /tmp/libk4abt1.0-dev_1.0.0_amd64.deb
RUN apt install -y libsoundio1
RUN dpkg -i /tmp/k4a-tools_1.3.0_amd64.deb
ENTRYPOINT ./bin/k4aviewer
