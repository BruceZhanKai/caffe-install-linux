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

- [Nvidia-GPU-Install-and-Setup](https://github.com/L706077/Nvidia-GPU-Install-and-Setup)
- [Installation Guide](https://github.com/BVLC/caffe/wiki/Ubuntu-16.04-or-15.10-Installation-Guide)
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

# USE_PKG_CONFIG := 1				---->	USE_PKG_CONFIG := 1 (if make fail on opencv)

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
$ cmake .. -DCMAKE_CXX_COMPILER=g++-5
$ make all -j8
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
```
add line in ~/caffe/CMakeLists.txt like opencv2.4.13
---->find_package(Qt5 COMPONENTS Core Gui Qml Quick Test Widgets OpenGL Concurrent REQUIRED)
```
- [Caffe fails on QT with errors](https://github.com/BVLC/caffe/issues/5155)
- [install linux qt5.9](https://www.jianshu.com/p/afbc42ad2cfd)
```
$ sudo apt-get install libxcb1 libxcb1-dev libx11-xcb1 libx11-xcb-dev libxcb-keysyms1
$ sudo apt-get install libxcb-keysyms1-dev libxcb-image0 libxcb-image0-dev libxcb-shm0
$ sudo apt-get install libxcb-shm0-dev libxcb-icccm4 libxcb-icccm4-dev libxcb-sync-dev
$ sudo apt-get install libxcb-xfixes0-dev libxrender-dev libxcb-shape0-dev libxcb-randr0-dev
$ sudo apt-get install libxcb-render-util0 libxcb-render-util0-dev libxcb-glx0-dev libxcb-xinerama0-dev
$ tar zxvf qt-everywhere-opensource-src-x.x.x.tar.gz
$ ./configure
(skip)$ ./configure -prefix "./build" -opensource -nomake tests -no-xcb -no-compile-examples
(skip)$ ./configure -confirm-license -opensource -debug-and-release -static -static-runtime -force-debug-info -opengl dynamic -prefix "./build" -qt-sqlite -qt-pcre -qt-zlib -qt-libpng -qt-libjpeg -opengl desktop -qt-freetype -nomake tests -no-compile-examples -nomake examples 
$ sudo make all -j8
$ sudo make install
$ vim .bashrc
---->export PATH="/xxx/xxx//Qtx.x.x/x.x/gcc/bin":$PATH
$ source .bashrc
```
- [opencv re-compile](https://blog.csdn.net/leijd10/article/details/25240013)


- issue 
```
../lib/libcaffe.so.1.0.0-rc3: undefined reference to `cv::imread(cv::String const&, int)'
```

- solve 
```
edit Makefile.config
#OPENCV_VERSION := 3		---->	OPENCV_VERSION := 3
```
```
edit CMakeLists.txt
set( OpenCV_DIR "/home/lili/bruce/github/opencv/release/" )	---->	# set( OpenCV_DIR "/home/lili/bruce/github/opencv/release/" )
```

- issue
```
../lib/libcaffe.so.1.0.0-rc3: undefined reference to `leveldb::DB::Open(leveldb::Options const&, std::string const&, leveldb::DB**)'
../lib/libcaffe.so.1.0.0-rc3: undefined reference to `google::base::CheckOpMessageBuilder::NewString()'
```
- solve
- [Caffe 1.0.0-rc3 make failed on Ubuntu 16.04 / CUDA 8.0](https://github.com/BVLC/caffe/issues/4492)
```
edit Makefile.config
# CUSTOM_CXX := g++			---->	CUSTOM_CXX := /usr/bin/g++-5
$ cmake .. -DCMAKE_CXX_COMPILER=g++-5
```

- issue
```
../lib/libcaffe.so.1.0.0-rc3: undefined reference to `google::protobuf::internal::AssignDescriptors
(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, google::protobuf::internal::MigrationSchema const*, google::protobuf::Message const* const*, unsigned int const*, google::protobuf::MessageFactory*, google::protobuf::Metadata*, google::protobuf::EnumDescriptor const**, google::protobuf::ServiceDescriptor const**)'

```
- solve
- [Undefined reference to google protobuf](https://github.com/BVLC/caffe/issues/3046)
- [protobuf install](https://blog.csdn.net/xiangxianghehe/article/details/78928629)
- [Caffe中的Protobuf版本问题](https://blog.csdn.net/phdsky/article/details/80994090)
```
$ wget https://github.com/google/protobuf/archive/v3.5.1.tar.gz
$ tar -xzvf v3.5.1.tar.gz
$ cd protobuf-3.5.1/
$ ./autogen.sh
$ ./configure --prefix=/usr/local/protobuf CC=/usr/bin/gcc
$ sudo make -j8 
$ sudo make install
$ sudo ldconfig
$ cd python/
$ sudo python2.7 setup.py build
$ sudo python2.7 setup.py install
$ sudo python2.7 setup.py test
```
```
edit Makefile.config
INCLUDE_DIRS := /usr/local/protobuf/include $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial /usr/include 
LIBRARY_DIRS := /usr/local/protobuf/lib $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial #/usr/local/matlab2016b/bin/glnxa64/
```
```
edit CMakeLists.txt
From
# ---[ Flags
if(UNIX OR APPLE)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -Wall")
endif()if()
To
# ---[ Flags
if(UNIX OR APPLE)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -Wall -std=c++11")
endif()if()
```
```
edit Makefile
CXXFLAGS += -pthread -fPIC $(COMMON_FLAGS) $(WARNINGS) -std=c++11
NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS) -std=c++11
LINKFLAGS += -pthread -fPIC $(COMMON_FLAGS) $(WARNINGS) -std=c++11
```
- [Ubuntu16.04多个版本GCC编译器的安装和切换](https://www.cnblogs.com/uestc-mm/p/7511063.html)
```
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 100
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 100
```
```
$ git clone https://github.com/google/glog
$ sudo apt-get install autoconf automake libtool
$ cd glog
$ ./autogen.sh 
$ ./configure 
$ ./configure CPPFLAGS="-I/usr/include -fPIC" LDFLAGS="-L/usr/lib/x86_64-linux-gnu/"
$ ./configure CPPFLAGS="-I/usr/include" LDFLAGS="-L/usr/lib/x86_64-linux-gnu/"
$ sudo make -j8
$ sudo make install
```
```
$ git clone https://github.com/gflags/gflags
$ cd gflags
$ cmake . 
$ sudo make -j8
$ sudo make install
```
```
$ sudo apt-get remove libprotobuf-dev
$ sudo apt-get remove protobuf-compiler
$ sudo apt-get remove python-protobuf
$ sudo rm -rf /usr/local/bin/protoc
$ sudo rm -rf /usr/bin/protoc
$ sudo rm -rf /usr/local/include/google
$ sudo rm -rf /usr/local/include/protobuf*
$ sudo rm -rf /usr/local/lib/libproto*
$ sudo rm -rf /usr/lib/libproto*
$ sudo rm -rf /usr/include/google
$ sudo rm -rf /usr/include/protobuf*
$ sudo rm -rf /usr/lib/x86_64-linux-gnu/libproto*

$ sudo apt-get update
$ sudo ldconfig
$ sudo apt-get install libprotobuf* protobuf-compiler python-protobuf
```


```
/home/superuser/anaconda3/pkgs/libprotobuf-3.5.2-0/include
/home/superuser/anaconda3/pkgs/libprotobuf-3.5.2-0/lib
```



- issue
```
[ 87%] Linking CXX shared library ../../lib/libcaffe.so
/usr/bin/ld: /usr/local/lib/libgflags.a(gflags.cc.o): relocation R_X86_64_32 against `.rodata.str1.1' can not be used when making a shared object; recompile with -fPIC
/usr/local/lib/libgflags.a: error adding symbols: Bad value
collect2: error: ld returned 1 exit status
src/caffe/CMakeFiles/caffe.dir/build.make:35923: recipe for target 'lib/libcaffe.so.1.0.0' failed
make[2]: *** [lib/libcaffe.so.1.0.0] Error 1
CMakeFiles/Makefile2:304: recipe for target 'src/caffe/CMakeFiles/caffe.dir/all' failed
make[1]: *** [src/caffe/CMakeFiles/caffe.dir/all] Error 2
Makefile:127: recipe for target 'all' failed
make: *** [all] Error 2
superuser@dcn-gpu-data-v-l-02:/home/lili/bruce/github/caffe/build$ 
```
```
re-install glog & gflags with -fPIC
edit CMakeCache.txt
--->CMAKE_CXX_FLAGS:STRING=-fPIC
$ cd ~/gflags/
$ mkdir build && cd build
$ export CXXFLAGS="-fPIC" && cmake .. && make VERBOSE=1
$ make -j4
$ make install
$ cd ~/caffe/build && cmake .. -DCMAKE_CXX_COMPILER=g++-5 && make clean && make all -j8
```

- issue
```
/home/lili/bruce/github/caffe/src/caffe/common.cpp: In function ‘void caffe::GlobalInit(int*, char***)’:
/home/lili/bruce/github/caffe/src/caffe/common.cpp:45:5: error: ‘::gflags’ has not been declared
   ::gflags::ParseCommandLineFlags(pargc, pargv, true);
     ^
[ 87%] Building CXX object src/caffe/CMakeFiles/caffe.dir/util/benchmark.cpp.o
src/caffe/CMakeFiles/caffe.dir/build.make:35131: recipe for target 'src/caffe/CMakeFiles/caffe.dir/common.cpp.o' failed
make[2]: *** [src/caffe/CMakeFiles/caffe.dir/common.cpp.o] Error 1
make[2]: *** Waiting for unfinished jobs....
CMakeFiles/Makefile2:304: recipe for target 'src/caffe/CMakeFiles/caffe.dir/all' failed
make[1]: *** [src/caffe/CMakeFiles/caffe.dir/all] Error 2
Makefile:127: recipe for target 'all' failed
make: *** [all] Error 2
```

- solve
- [Can't build with error ‘::gflags’ has not been declared](https://github.com/BVLC/caffe/issues/2597)
```
$ export CXXFLAGS="-fPIC" && cmake .. -DBUILD_SHARED_LIBS=ON -DBUILD_STATIC_LIBS=ON -DBUILD_gflags_LIB=ON
$ sudo make install
```

- [Glog安裝與卸載](https://blog.csdn.net/u012348774/article/details/80620685)

- [caffe:cmake编译指定glog,gflag路径](https://blog.csdn.net/10km/article/details/72967656)

```
$ cmake ..  -G "Unix Makefiles" \
    -DGLOG_ROOT_DIR=/home/lili/bruce/glog \
    -DGFLAGS_ROOT_DIR=/home/lili/bruce/gflags \
	-DCMAKE_CXX_COMPILER=g++-5
	```



