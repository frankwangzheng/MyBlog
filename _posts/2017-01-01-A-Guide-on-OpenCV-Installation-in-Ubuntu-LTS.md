---
layout: post
title: OpenCV Installation in Ubuntu LTS
subtitle: A Personal Guide
tags: [Library]
bigimg: /img/path.jpg
---

This article is my step-by-step guide to install OpenCV 3.x and OpenCV 2.4.13.x in Ubuntu 16.04&14.04. It is advisable to download [OpenCV3.x](https://github.com/opencv/opencv/tree/3.2.0) and its corresponding [OpenCV_contrib](https://github.com/opencv/opencv_contrib/tree/3.2.0) directly from Github. There is no corresponding OpenCV_contrib for [OpenCV2.4](https://github.com/opencv/opencv/tree/2.4.13.2).

### Step 1: Clean Up System
```
sudo apt-get remove --purge libopencv-dev
sudo apt-get autoremove
```
### Step 2: Update System
```
sudo apt-get update   
sudo apt-get upgrade
```
### Step 3: Install Dependencies
#### Build Tools:
```
sudo apt-get install build-essential pkg-config git
sudo apt-get install unzip bzip2 libbz2-dev 
sudo apt-get install cmake cmake-qt-gui 
```
Note: For Ubuntu 14.04, install cmake3.2 using [ppa](http://askubuntu.com/questions/610291/how-to-install-cmake-3-2-on-ubuntu-14-04)

#### GUI and Visualization:
For Opencv 3.x:
```
sudo apt-get install libgtk-3-dev
sudo apt-get install qt5-default qtbase5-dev libqt5opengl5
sudo apt-get install libvtk6-dev libvtk6-qt-dev
```
For OpenCV 2.4.13.x:
```
sudo apt-get install libgtk2.0-dev
sudo apt-get install libqt4-dev libqt4-opengl-dev
sudo apt-get install libvtk5-dev libvtk5-qt4-dev
```

#### Image I/O:
```
sudo apt-get install libjpeg-dev libjasper-dev libpng12-dev libtiff5-dev libwebp-dev libopenexr-dev
```

#### Video I/O:
```
sudo apt-get install ffmpeg lidavcodec-dev libavformat-dev libavresample-dev libswscale-dev libswresample-dev
sudo apt-get install libtheora-dev 
sudo apt-get install libxvidcore-dev
sudo apt-get install v4l-utils libv4l-dev
sudo apt-get install x264 libx264-dev 
sudo apt-get install x265 libx265-dev
sudo apt-get install libvpx3 libvpx-dev
```
Note: For Ubuntu 14.04, install ffmpeg through [ppa](https://launchpad.net/~mc3man/+archive/ubuntu/trusty-media)

#### AUDIO I/O:
```
sudo apt-get install libopencore-amrnb-dev libopencore-amrwb-dev
sudo apt-get install libfaac-dev
sudo apt-get install libmp3lame-dev
sudo apt-get install libvorbis-dev
sudo apt-get install libopus-dev
```
Note: For Ubuntu 14.04, libfaac-dev is not available. 

#### Mutimedia Support:
```
sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
sudo apt-get install libxine2-dev
```

Older Version(Optional):
```
sudo apt-get install libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev
```

#### Parallelism and Linear Algebra
``` 
sudo apt-get install libtbb-dev
sudo apt-get install libeigen3-dev
sudo apt-get install libatlas-base-dev libblas-dev liblapack-dev
```

#### Camera I/O
```
sudo apt-get install libdc1394-22 libdc1394-22-dev
sudo apt-get install libunicap2-dev
sudo apt-get install libgphoto2-dev
```

#### Fortran
```
sudo apt-get install gfortran
```

#### Python
```
sudo apt-get install --reinstall python-dev python-numpy
sudo apt-get install python3-dev python3-numpy
```

#### OpenCL
```
sudo apt-get install libclblas2 libclfft2 libclfft-dev libclblas-dev
sudo apt-get install ocl-icd-libopencl-dev
```

#### OpenGL
```
sudo apt-get install libgl1-mesa-dev mesa-common-dev
```

#### Java
```
sudo apt-get install ant 
sudo apt-get install default-jdk default-jre
sudo apt install openjdk-8-jdk openjdk-8-jre
```
#### Matlab
MATLAB needs to be installed using its ISO file. Run:
```
sudo mkdir /media/matlab
sudo mount -o loop matlab.xxx.iso /media/matlab
cd /media/matlab
sudo ./install
sudo umount /media/matlab
sudo rm -rf /media/matlab
```
After installation, create a soft-link for the executable:
```
sudo ln -s -f /usr/local/MATLAB/.../bin/matlab /usr/lib/matlab
```
Run MATLAB using:
```
sudo matlab
```
In Ubuntu 16.04, MATLAB may crash after startup, fix that with:
```
sudo apt-get install matlab-support
```

#### Documentations
```
sudo apt-get install doxygen
sudo apt-get install sphinx-common 
(sudo apt-get install --no-install-recommends texlive-latex-base texlive-latex-extra)
```

#### Others
```
sudo apt-get install yasm
sudo apt-get install libgdal-dev
```

### Step 4: Compile and Install OpenCV
Unzip tar.gz file or zip file
```
tar -xvf opencv-x.x.x.tar.gz -C ~/
unzip opencv-x.x.x.zip -d ~/
```
Inside downloaded OpenCV folder:
```
cd ~/opencv-x.x.x
mkdir build
cd build
```

Then invoke cmake-gui by typing "cmake-gui" in terminal. It is a convenient way to generate makefile, then do:

```
make -j $(nproc)
sudo make install
```

Note that if using NVIDIA GPU card, the `CUDA_ARCH_PTX` should match the card's CUDA Capability.

### Step 5: Error Debug

#### Error 1: Cmake cannot find python libraries(Ubuntu 14.04)
Cmake 2.8.* will invoke this error. Upgrade the cmake using this [ppa](http://askubuntu.com/questions/610291/how-to-install-cmake-3-2-on-ubuntu-14-04). 

#### Error 2: Cmake cannot find CUDA(FindCUDA.cmake)
Add the following lines in the ~/.bashrc:
```
export CUDA_HOME=/usr/local/cuda-x.x 
export LD_LIBRARY_PATH=${CUDA_HOME}/lib64 
export PATH=${CUDA_HOME}/bin:${PATH} 
export CUDA_BIN_PATH=$CUDA_HOME #Stated in FindCUDA.cmake
```
Then execute:
```
source ~/.bashrc
```
or manually define **CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-x.x** in cmake-gui

#### Error 3: Undefined "list_filterout" macro for cmake
Solve it by doing [this](https://github.com/pld-linux/opencv/commit/dadee4672641272b129410bc097f5c199bb4fb43)

#### Error 4: "NppiGraphcutState" has not been delared in "graphcuts.cpp"(CUDA 8.0)
Solve it by doing [this](https://github.com/opencv/opencv/pull/6510/files)

### Step 6: Configure Environment
Execute the following:
```
sudo /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'
sudo ldconfig
```
then add the following lines in ~/.bashrc:
```
export PKG_CONFIG_PATH="$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig"
```

### Step 7: Check Install
To check the include path, execute:
```
pkg-config --cflags opencv
```
To check the lib path, execute: 
```
pkg-config --libs opencv
```
Usually the include and library paths are: **/usr/local/include/opencv** and **/usr/local/lib**

### Step 8: Test an Example
To test a C example, connect the PC with a webcam and execute:
```
cd opencv-x.x.x/build/bin/
./c-example-facedetect
```

## Manual Uninstallation
Go to "build" folder where makefile resides, do:
```
sudo make uninstall
```

## Awesome OpenCV Installation Reference
[PyImageSearch's Installation Guide](http://www.pyimagesearch.com/2016/10/24/ubuntu-16-04-how-to-install-opencv/)
[Caffe's Github Wiki Guide](https://github.com/BVLC/caffe/wiki/OpenCV-3.1-Installation-Guide-on-Ubuntu-16.04)  