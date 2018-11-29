# caffe-install-linux

## verify package installation

### GPU

```
$ lspci | grep -i NVIDIA
```
```
$ nvidia-smi
```

### cuda

```
$ nvcc -V
->nvcc: NVIDIA (R) Cuda compiler driver
->Copyright (c) 2005-2017 NVIDIA Corporation
->Built on Fri_Sep__1_21:08:03_CDT_2017
->Cuda compilation tools, release 9.0, V9.0.176
```
```
or
$ cat /usr/local/cuda/version.txt
->CUDA Version 9.0.176
```

### cudnn

```
$ cat /usr/include/cudnn.h | grep CUDNN_MAJOR -A 2
->#define CUDNN_MAJOR 7
->#define CUDNN_MINOR 3
->#define CUDNN_PATCHLEVEL 1
->--
->#define CUDNN_VERSION (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)
->
->#include "driver_types.h"
```

### opencv

```
$ pkg-config opencv --libs
```
```
$ pkg-config opencv --modversion
->2.4.9.1
```
```
$ sudo find / -iname "*opencv*"
$ sudo find / -iname "*opencv*" > /home/ubuntu/Desktop/opencv_find.txt
```
```
or
$ python
>>> import cv2
>>> cv2.__version__
->'3.1.0'
```

## Install

### opencv

- [cuda9.0&opencv2.4.13](https://blog.csdn.net/zhuangwu116/article/details/81136117)

- [cuda9.0&opencv3.1.0](https://blog.csdn.net/fk2016/article/details/83589726)

### matlab 

- [ubuntu16.04&matlab2016b](https://blog.csdn.net/Jesse_Mx/article/details/53956358)
- [ubuntu-non-UI&matlab2016b](https://blog.csdn.net/zhaohuihuihi/article/details/77455715)
- [ubuntu-non-UI&matlab2017b](https://blog.csdn.net/Xiao_Song_PKU/article/details/82700228)
- mount 1st iso file
```
$ cd /home/lili/bruce
$ mkdir matlab 
$ sudo mount -t auto -o loop R2016b_glnxa64_dvd1.iso matlab/
```
- install
```
$ sudo ./matlab/install -mode silent -fileInstallationKey 09806-07443-53955-64350-21751-41297 -agreeToLicense yes -activationPropertiesFile /home/lili/bruce/Matlab2016blinux64crack/license_standalone.lic -destinationFolder /usr/local/matlab2016b
[reference]
$ ./install -mode silent -fileInstallationKey 09806-07443-53955-64350-21751-41297 -agreeToLicense yes 
  -activationPropertiesFile /home/gpu-server02/software/MATLAB_R2017b_Linux/MATLABR2017b_Linux_Crack/activate.ini 
  -destinationFolder /usr/local/matlab2017b
[reference]
$ ./install -mode silent  -fileInstallationKey 25716-63335-16746-06072 -agreeToLicense yes 
  - activationPropertiesFile home/usr/XXX/crack/license_standalone.lic 
  -destinationFolder home/usr/matlab2016
```
- open another new terminal to mount 2nd iso file
```
$ sudo mount -t auto -o loop Linux/R2016b_glnxa64_dvd2.iso matlab/
```
- activate again for safety(skip)
```
$ cd /usr/local/matlab2016b/bin
$ ./activate_matlab.sh -propertiesFile /home/gpu-server02/software/MATLAB_R2017b_Linux/MATLABR2017b_Linux_Crack/activate.ini
```
- crack
```
$ sudo cp /home/lili/bruce/Matlab2016blinux64crack/R2016b/bin/glnxa64/* /usr/local/matlab2016b/bin/glnxa64
$ sudo mkdir /usr/local/matlab2016b/licenses
$ sudo cp /home/lili/bruce/Matlab2016blinux64crack/license_standalone.lic /usr/local/matlab2016b/licenses

[reference]
$ cp home/usr/XXX/crack/R2016b/bin/glnxa64/* home/usr/matlab2016/bin/glnxa64
$ mkdir home/usr/matlab2016/licenses
$ cp home/usr/XXX/crack/license_standalone.lic  home/usr/matlab2016/licenses
[reference]
$ rm -rf /usr/local/matlab2017b/bin/glnxa64/libmwservices.so
$ cp /home/gpu-server02/software/MATLAB_R2017b_Linux/MATLABR2017b_Linux_Crack/libmwservices.so /usr/local/matlab2017b/bin/glnxa64/
$ cp /home/gpu-server02/software/MATLAB_R2017b_Linux/MATLABR2017b_Linux_Crack/license_standalone.lic /usr/local/matlab2017b/bin/licenses/
$ umount -l /media/matlab/
```
- execute matlab
```
$ cd /usr/local/matlab2016b/bin
$ ./matlab -c /usr/local/matlab2016b/licenses/license_standalone.lic
```

- system setting
```
$ vim ~/.bashrc
export PATH=/usr/local/matlab2016b/bin:$PATH
$ source ~/.bashrc
```


### caffe

- [指定python](https://www.cnblogs.com/TiBAi/p/6848307.html)
- [Caffe Matlab接口](https://blog.csdn.net/yangguangqizhi/article/details/74174335)

1. requirements
```
ubuntu 16.04
opencv 2.4.13
python 2.7
matlab 2016b
cuda 9.0
cudnn 7.3
```
2. prerequisites
```
$ git clone https://github.com/Adnan1011/NR-IQA-CNN.git
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
$ sudo apt-get install --no-install-recommends libboost-all-dev
$ sudo apt-get install libatlas-base-dev 
$ sudo apt-get install libopenblas-dev
$ sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
$ sudo apt-get install -y python-pip
//=== python2.7 ===
$ sudo apt-get install -y python-dev
$ sudo apt-get install -y python-numpy python-scipy
//=== python3.5 ===
$ sudo apt-get install -y python3-dev
$ sudo apt-get install -y python3-numpy python3-scipy
//=================
$ cd caffe/python/
$ for req in $(cat requirements.txt); do sudo -H pip install $req --upgrade; done
$ sudo apt-get update
$ cp Makefile.config.example Makefile.config

Open Makefile.config to change:

OPENCV_VERSION := 3					---->	# OPENCV_VERSION := 3

# USE_CUDNN := 1   					---->   USE_CUDNN := 1

# CUDA_DIR := /usr/local/cuda   	---->   CUDA_DIR := /usr/local/cuda-9.0

CUDA_ARCH :=    ...
                -gencode arch=compute_70,code=sm_70 \
                -gencode arch=compute_70,code=compute_70

#MATLAB_DIR := /usr/local			---->	MATLAB_DIR := /usr/local/matlab2016b

PYTHON_INCLUDE := /usr/include/python2.7 \
                /usr/lib/python2.7/dist-packages/numpy/core/include
				
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial

LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial

***attantion to check PYTHON_INCLUDE is /usr/lib/python2.7 or /usr/local/lib/python2.7***
```

3. edit Makefile
```
$ vim ./Makefile
NVCCFLAGS += -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
---->NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
```
4. edit CMakeLists.txt to add
```
# ---[ Includes
---->set(${CMAKE_CXX_FLAGS} "-D_FORCE_INLINES ${CMAKE_CXX_FLAGS}")

---->set( OpenCV_DIR "/home/lili/bruce/github/opencv/release/" )
---->find_package(OpenCV REQUIRED)
```

5. make caffe
```
$ mkdir build
$ cd build
$ cmake ..
$ make all
$ make install
$ make runtest
```

- issue
```
superuser@dcn-gpu-data-v-l-02:/home/lili/bruce/github/NR-IQA-CNN/build$ make all
[  1%] Built target proto
[  1%] Building NVCC (Device) object src/caffe/CMakeFiles/cuda_compile.dir/solvers/cuda_compile_generated_adadelta_solver.cu.o
/home/lili/bruce/github/NR-IQA-CNN/include/caffe/util/cudnn.hpp(112): error: too few arguments in function call

1 error detected in the compilation of "/tmp/tmpxft_0000565f_00000000-4_adadelta_solver.cpp4.ii".
CMake Error at cuda_compile_generated_adadelta_solver.cu.o.cmake:266 (message):
  Error generating file
  /home/lili/bruce/github/NR-IQA-CNN/build/src/caffe/CMakeFiles/cuda_compile.dir/solvers/./cuda_compile_generated_adadelta_solver.cu.o

src/caffe/CMakeFiles/caffe.dir/build.make:547: recipe for target 'src/caffe/CMakeFiles/cuda_compile.dir/solvers/cuda_compile_generated_adadelta_solver.cu.o' failed
make[2]: *** [src/caffe/CMakeFiles/cuda_compile.dir/solvers/cuda_compile_generated_adadelta_solver.cu.o] Error 1
CMakeFiles/Makefile2:272: recipe for target 'src/caffe/CMakeFiles/caffe.dir/all' failed
make[1]: *** [src/caffe/CMakeFiles/caffe.dir/all] Error 2
Makefile:127: recipe for target 'all' failed
make: *** [all] Error 2
superuser@dcn-gpu-data-v-l-02:/home/lili/bruce/github/NR-IQA-CNN/build$.
```
- solve
- [cuda多版本切換](https://blog.csdn.net/Maple2014/article/details/78574275)
- [cudnn多版本切換](https://blog.csdn.net/zhaishengfu/article/details/52333674)
- [cudnn安裝、路徑、版本](https://blog.csdn.net/ture_dream/article/details/52677619)
```
1. find the actaully cudnn path if there are not in /usr/local/cuda
2. go to this [caffe/../cudnn.hpp](https://github.com/BVLC/caffe/blob/master/include/caffe/util/cudnn.hpp) 
 and Copy this and replace the old one
```

- issue
```
[ 80%] Linking CXX shared library ../../lib/libcaffe.so
/usr/bin/ld: cannot find -lQt5::Core
/usr/bin/ld: cannot find -lQt5::Gui
/usr/bin/ld: cannot find -lQt5::Widgets
/usr/bin/ld: cannot find -lQt5::Test
/usr/bin/ld: cannot find -lQt5::Concurrent
/usr/bin/ld: cannot find -lQt5::OpenGL
collect2: error: ld returned 1 exit status
src/caffe/CMakeFiles/caffe.dir/build.make:4871: recipe for target 'lib/libcaffe.so.1.0.0-rc3' failed
make[2]: *** [lib/libcaffe.so.1.0.0-rc3] Error 1
CMakeFiles/Makefile2:272: recipe for target 'src/caffe/CMakeFiles/caffe.dir/all' failed
make[1]: *** [src/caffe/CMakeFiles/caffe.dir/all] Error 2
Makefile:127: recipe for target 'all' failed
make: *** [all] Error 2
```
- solve

- [linux qt5.9](https://www.jianshu.com/p/afbc42ad2cfd)
```
$ sudo apt-get install libxcb1 libxcb1-dev libx11-xcb1 libx11-xcb-dev libxcb-keysyms1
$ sudo apt-get install libxcb-keysyms1-dev libxcb-image0 libxcb-image0-dev libxcb-shm0
$ sudo apt-get install libxcb-shm0-dev libxcb-icccm4 libxcb-icccm4-dev libxcb-sync-dev
$ sudo apt-get install libxcb-xfixes0-dev libxrender-dev libxcb-shape0-dev libxcb-randr0-dev
$ sudo apt-get install libxcb-render-util0 libxcb-render-util0-dev libxcb-glx0-dev libxcb-xinerama0-dev
$ tar zxvf qt-everywhere-opensource-src-x.x.x.tar.gz
$ ./configure
$ ./configure -prefix "./build" -opensource -nomake tests
$ ./configure -confirm-license -opensource -debug-and-release -static -static-runtime -force-debug-info -opengl dynamic -prefix "./build" -qt-sqlite -qt-pcre -qt-zlib -qt-libpng -qt-libjpeg -opengl desktop -qt-freetype -nomake tests -no-compile-examples -nomake examples 
$ sudo make all -j8
$ sudo make install
$ vim .bashrc
---->export PATH="/xxx/xxx//Qtx.x.x/x.x/gcc/bin":$PATH
$ source .bashrc
```









