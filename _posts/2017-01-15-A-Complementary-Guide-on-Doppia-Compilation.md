---
layout: post
title: Doppia Compilation in Ubuntu LTS with CUDA8.0 
subtitle: A Personal Guide
tags: [Library]
bigimg: /img/path.jpg
---

It is a complementary guide to compile&run Rodrigo Benenson's [doppia](https://bitbucket.org/rodrigob/doppia) repository in Ubuntu16.04 with CUDA8.0&OpenCV2.4.13. Most of the setup steps can be found in the official guide.

#### Library Setup
To install OpenCV2.4.13, please refer to my [guide](http://frankwangzheng.me/2017-01-01-A-Guide-on-OpenCV-Installation-in-Ubuntu-LTS/). Other libraries, including boost1.58, sdl1.2 and google-protobuf2.6.1 can be installed through `apt-get`:

```shell
sudo apt-get install libboost-all-dev
sudo apt-get install libsdl1.2-dev
sudo apt-get install libprotobuf-dev libprotoc-dev python-protobuf protobuf-compiler
```

Note: Do not install `libsdl2-dev`, otherwise "doppia" cannot find SDL files

Other important libraries include cmake3.5.1 and g++5.4.0

#### `common_setting.cmake` Edit

It is the section in the `common_setting.cmake` that matters: 

```
elseif(${HOSTNAME} STREQUAL  "ZHWANG-LINUX")
  # change the_name_of_your_machine to what /bin/hostname returns

  message(STATUS "Using ZHWANG-LINUX compilation options")
  # start with an empty section, and see what fails as you go through the readme.text instructions

  option(USE_GPU "Should the GPU be used ?" True)

  set(CUDA_BUILD_CUBIN OFF)  
  set(local_CUDA_LIB_DIR "/usr/local/cuda-8.0/lib64")  
  set(CUDA_INCLUDE_DIRS "/usr/local/cuda-8.0/include")  
  set(cuda_LIBS "")
```

#### Error Debug

**Error 1**: `/usr/local/include/boost/variant/get.hpp:178:5: error: invalid application of ‘sizeof’ to incomplete type ‘boost::STATIC_ASSERTION_FAILURE<false>’` ([Issue #84](https://bitbucket.org/rodrigob/doppia/issues/84/the-problem-while-compiling))

**Solution**: This error is due to boost 1.58. Change line in common_settings.cmake from `set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall ")` to `set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -DBOOST_VARIANT_USE_RELAXED_GET_BY_DEFAULT=1")`


**Error 2**: `doppia/src/objects_detection/cascade_stages/SoftCascadeOverIntegralChannelsFastFractionalStage.cpp:24:9: 
error: ‘swap’ is not a member of ‘std’`

**Solution**: add `#include <iostream>` in `doppia/src/objects_detection/cascade_stages/SoftCascadeOverIntegralChannelsFastFractionalStage.hpp`

**Error 3(runtime)**: `A std::exception was raised: This executable was compiled without support for GpuVeryFastIntegralChannelsDetector
terminate called after throwing an instance of 'std::runtime_error'
  what():  This executable was compiled without support for GpuVeryFastIntegralChannelsDetector
Aborted (core dumped)`

**Solution**: make sure line `option(USE_GPU "Should the GPU be used ?" True)` is in the user-specific section in `common_setting.cmake`





