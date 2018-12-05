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
---
## OpenCV Install

- [cuda9.0&opencv2.4.13](https://blog.csdn.net/zhuangwu116/article/details/81136117)

- [cuda9.0&opencv3.1.0](https://blog.csdn.net/fk2016/article/details/83589726)
### Install Dependencies
```
$ sudo apt-get update
$ sudo apt-get install -y build-essential
$ sudo apt-get install -y cmake
$ sudo apt-get install -y libgtk2.0-dev
$ sudo apt-get install -y pkg-config
$ sudo apt-get install -y python-numpy python-dev
$ sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev
$ sudo apt-get install -y libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev
$ sudo apt-get -qq install libopencv-dev build-essential checkinstall cmake pkg-config yasm libjpeg-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libxine2 libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev python-dev python-numpy libtbb-dev libqt4-dev libgtk2.0-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 v4l-utils
```
### Compile and Install
```
$ wget http://downloads.sourceforge.net/project/opencvlibrary/opencv-unix/2.4.13/opencv-2.4.13.zip
$ unzip opencv-2.4.13.zip
$ cd opencv-2.4.13
$ mkdir release
$ cd release
$ cmake -DCMAKE_BUILD_TYPE=RELEASE -DCMAKE_INSTALL_PREFIX=/usr/local -DWITH_QT=ON -DWITH_OPENGL=ON -DWITH_TBB=ON -DBUILD_NEW_PYTHON_SUPPORT=ON -DWITH_V4L=ON INSTALL_C_EXAMPLES=ON -DINSTALL_PYTHON_EXAMPLES=ON -DBUILD_EXAMPLES=ON -DWITH_XINE=ON -DINSTALL_TESTS=ON -DWITH_GSTREAMER=ON -DWITH_CUDA=OFF -DBUILD_EXAMPLES=ON ..
$ make all -j8
$ make install

```
### (skip)Environment Startup  
```
$ sudo gedit /etc/ld.so.conf.d/opencv.conf
  /usr/local/lib  
$ sudo ldconfig

$ sudo gedit /etc/bash.bashrc 
    PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig  
    export PKG_CONFIG_PATH
-----------------------------------------------
$ su //获取root权限，否则下面的source命令不可用  
  su: Authentication failure  
$ sudo passwd root
  Enter new UNIX password:   
  Retype new UNIX password:   
  passwd: password updated successfully 
-----------------------------------------------
$ su

# source /etc/bash.bashrc 
然后按 Ctrl+d 组合键来退出root权限，接着输入下面的命令即可：

$ sudo updatedb
```




---
## Matlab Install

- [ubuntu16.04&matlab2016b](https://blog.csdn.net/Jesse_Mx/article/details/53956358)
- [ubuntu-non-UI&matlab2016b](https://blog.csdn.net/zhaohuihuihi/article/details/77455715)
- [ubuntu-non-UI&matlab2017b](https://blog.csdn.net/Xiao_Song_PKU/article/details/82700228)
mount 1st iso file  
```
$ cd /home/lili/bruce
$ mkdir matlab 
$ sudo mount -t auto -o loop R2016b_glnxa64_dvd1.iso matlab/
```
### install
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
(skip)activate again for safety  
```
$ cd /usr/local/matlab2016b/bin
$ ./activate_matlab.sh -propertiesFile /home/gpu-server02/software/MATLAB_R2017b_Linux/MATLABR2017b_Linux_Crack/activate.ini
```
### crack
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
### execute matlab
```
$ cd /usr/local/matlab2016b/bin
$ ./matlab -c /usr/local/matlab2016b/licenses/license_standalone.lic
```

### system setting
```
$ vim ~/.bashrc
export PATH=/usr/local/matlab2016b/bin:$PATH
$ source ~/.bashrc
```

---
## Caffe Install

- [Nvidia-GPU-Install-and-Setup](https://github.com/L706077/Nvidia-GPU-Install-and-Setup)
- [Installation Guide](https://github.com/BVLC/caffe/wiki/Ubuntu-16.04-or-15.10-Installation-Guide)
- [指定python](https://www.cnblogs.com/TiBAi/p/6848307.html)
- [Caffe Matlab接口](https://blog.csdn.net/yangguangqizhi/article/details/74174335)

### requirements
```
ubuntu 16.04
opencv 2.4.13
python 2.7
matlab 2016b
cuda 9.0
cudnn 7.3
```
### prerequisites
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

### edit Makefile
```
$ vim ./Makefile
NVCCFLAGS += -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
---->NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
```
### edit CMakeLists.txt to add
```
# ---[ Includes
---->set(${CMAKE_CXX_FLAGS} "-D_FORCE_INLINES ${CMAKE_CXX_FLAGS}")

---->set( OpenCV_DIR "/home/lili/bruce/github/opencv/release/" )
---->find_package(OpenCV REQUIRED)
```

### make caffe
```
$ mkdir build
$ cd build
$ cmake ..
$ cmake .. -DCMAKE_CXX_COMPILER=g++-5
$ make all -j8
$ make install
$ make runtest
```
---
## All Caffe Issue

### Cuda - error: too few arguments in function call
- Show Error
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
- Solve
- [cuda多版本切換](https://blog.csdn.net/Maple2014/article/details/78574275)
- [cudnn多版本切換](https://blog.csdn.net/zhaishengfu/article/details/52333674)
- [cudnn安裝、路徑、版本](https://blog.csdn.net/ture_dream/article/details/52677619)

1. find the actaully cudnn path if there are not in /usr/local/cuda  
2. go to this [caffe/../cudnn.hpp](https://github.com/BVLC/caffe/blob/master/include/caffe/util/cudnn.hpp)   
and Copy this and replace the old one  

### Qt - cannot find -lQt5::Core
- Show Error
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
- Solve
add line in ~/caffe/CMakeLists.txt like opencv2.4.13  
```
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


### Caffe - undefined reference to `cv::imread 
- Show Error
```
$ make all -j8
...
../lib/libcaffe.so.1.0.0-rc3: undefined reference to `cv::imread(cv::String const&, int)'
...
```

- Solve 

	Maybe opencv2.4.13 I installed is not complete with python2.7 or ...
	So I use other version already installed to do $ make all -j8
	
	Edit Makefile.config
```
#OPENCV_VERSION := 3		---->	OPENCV_VERSION := 3
```
	Edit CMakeLists.txt
```
set( OpenCV_DIR "/home/lili/bruce/github/opencv/release/" )	---->	#set( OpenCV_DIR "/home/lili/bruce/github/opencv/release/" )
```

### Leveldb - undefined reference to `leveldb::DB
- Show Error
```
../lib/libcaffe.so.1.0.0-rc3: undefined reference to `leveldb::DB::Open(leveldb::Options const&, std::string const&, leveldb::DB**)'
../lib/libcaffe.so.1.0.0-rc3: undefined reference to `google::base::CheckOpMessageBuilder::NewString()'
```
- Solve
- [Caffe 1.0.0-rc3 make failed on Ubuntu 16.04 / CUDA 8.0](https://github.com/BVLC/caffe/issues/4492)  
Edit Makefile.config  
```
# CUSTOM_CXX := g++			---->	CUSTOM_CXX := /usr/bin/g++-5
$ cmake .. -DCMAKE_CXX_COMPILER=g++-5
```
---

## All Caffe Issue of Protobuf


以下Issue，斟酌參考，首先

gcc和g++對齊到version 5.4.0 20160609 ->失敗 undefined reference to google::protobuf

下了protobuf3.5.1，用源碼安裝再make -> 失敗 undefined reference to google::protobuf

接著下了glog和gflags，用源碼安裝再make ->失敗 undefined reference to google::protobuf

遇到gflags源碼安裝的其他問題，修改common.hpp問題消失但make還是 ->失敗 undefined reference to google::protobuf

發現caffe cmake時，glog、gflags、protbuf的路徑無法被修改

接著改~/caffe/build/CMakeCache.txt的glog、gflags、protbuf的路徑

路徑成功修改但make還是 ->失敗 undefined reference to google::protobuf

最後查看server上不只3.5.1版本，在另一個anaconda上也有3.5.1和3.4.0

有人說不同版本共存會有衝突，將不同版本的protbuf刪除再編譯

礙於多人共同使用server，我沒有這樣做

正要考慮直接用anaconda創一個新環境來搞

最後在放棄之前下了protobuf3.6.0及3.3.0

先刪除3.5.1，$ sudo rm -rf /usr/local/protobuf，接著裝3.6.0，make -> 幹終於成功了

以下是所有坑，傷眼睛斟酌看


### Caffe - undefined reference to `google::protobuf
- Show Error
```
../lib/libcaffe.so.1.0.0-rc3: undefined reference to `google::protobuf::internal::AssignDescriptors
(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, google::protobuf::internal::MigrationSchema const*, google::protobuf::Message const* const*, unsigned int const*, google::protobuf::MessageFactory*, google::protobuf::Metadata*, google::protobuf::EnumDescriptor const**, google::protobuf::ServiceDescriptor const**)'
```
- Solve
- [Undefined reference to google protobuf](https://github.com/BVLC/caffe/issues/3046)
- [protobuf install](https://blog.csdn.net/xiangxianghehe/article/details/78928629)
- [Caffe中的Protobuf版本问题](https://blog.csdn.net/phdsky/article/details/80994090)

源碼安裝protobuf  
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

Edit Makefile.config 讓protobuf的路徑擺最前面  
```
INCLUDE_DIRS := /usr/local/protobuf/include $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial /usr/include 
LIBRARY_DIRS := /usr/local/protobuf/lib $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial #/usr/local/matlab2016b/bin/glnxa64/
```

Edit CMakeLists.txt  
```
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

Edit Makefile  
```
CXXFLAGS += -pthread -fPIC $(COMMON_FLAGS) $(WARNINGS) -std=c++11
NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS) -std=c++11
LINKFLAGS += -pthread -fPIC $(COMMON_FLAGS) $(WARNINGS) -std=c++11
```
- [Ubuntu16.04多个版本GCC编译器的安装和切换](https://www.cnblogs.com/uestc-mm/p/7511063.html)

對齊gcc g++版本  
```
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 100
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 100
```
- [安装glog和gflags](https://blog.csdn.net/csm201314/article/details/75094527)
- [GLog & GFlags 的安装](http://www.liuxiao.org/2018/04/glog-gflags-%E7%9A%84%E5%AE%89%E8%A3%85/)

glog install from source  
```
$ git clone https://github.com/google/glog
$ sudo apt-get install autoconf automake libtool
$ cd glog
$ ./autogen.sh 
$ ./configure CPPFLAGS="-I/usr/local/include -fPIC" LDFLAGS="-L/usr/local/lib"
$ sudo make -j8
$ sudo make install
```
gflags install from source  
```
$ git clone https://github.com/gflags/gflags
$ cd gflags
$ cmake . 
$ sudo make -j8
$ sudo make install
```
(skip)re-install protobuf相關package  
```
(skip)$ sudo apt-get remove libprotobuf-dev
(skip)$ sudo apt-get remove protobuf-compiler
(skip)$ sudo apt-get remove python-protobuf
(skip)$ sudo rm -rf /usr/local/bin/protoc
(skip)$ sudo rm -rf /usr/bin/protoc
(skip)$ sudo rm -rf /usr/local/include/google
(skip)$ sudo rm -rf /usr/local/include/protobuf*
(skip)$ sudo rm -rf /usr/local/lib/libproto*
(skip)$ sudo rm -rf /usr/lib/libproto*
(skip)$ sudo rm -rf /usr/include/google
(skip)$ sudo rm -rf /usr/include/protobuf*
(skip)$ sudo rm -rf /usr/lib/x86_64-linux-gnu/libproto*

(skip)$ sudo apt-get update
(skip)$ sudo ldconfig
(skip)$ sudo apt-get install libprotobuf* protobuf-compiler python-protobuf
```

### Caffe - can not be used when making a shared object; recompile with -fPIC
- Show Error
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
- Solve - re-install glog & gflags with -fPIC  
Edit glfags/build/CMakeCache.txt  
```
--->CMAKE_CXX_FLAGS:STRING=-fPIC
$ cd ~/gflags/
$ mkdir build && cd build
$ export CXXFLAGS="-fPIC" && cmake .. && make VERBOSE=1
$ make -j4
$ make install
$ cd ~/caffe/build && cmake .. -DCMAKE_CXX_COMPILER=g++-5 && make clean && make all -j8
```

### Caffe - error: ‘::gflags’ has not been declared
- Show Error
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

- Solve
- [Can't build with error ‘::gflags’ has not been declared](https://github.com/BVLC/caffe/issues/2597)

```
(skip)$ cd ~/gflags/
(skip)$ mkdir build && cd build
(skip)$ export CXXFLAGS="-fPIC" && cmake .. -DBUILD_SHARED_LIBS=ON -DBUILD_STATIC_LIBS=ON -DBUILD_gflags_LIB=ON
(skip)$ sudo make -j4
(skip)$ sudo make install
```
Edit in the file include/caffe/common.hpp  
```
#ifndef GFLAGS_GFLAGS_H_			--->	//#ifndef GFLAGS_GFLAGS_H_
namespace gflags = google;
#endif  // GFLAGS_GFLAGS_H_			--->	//#endif  // GFLAGS_GFLAGS_H_
```

- [Glog安裝與卸載](https://blog.csdn.net/u012348774/article/details/80620685)

- [caffe:cmake编译指定glog,gflag路径](https://blog.csdn.net/10km/article/details/72967656)  

指定glog,gflag路径，至~/caffe/build/CMakeCache.txt修改glog,gflag,protobuf路徑  
```
$ vim ~/caffe/build/CMakeCache.txt
//Path to a file.
GLOG_INCLUDE_DIR:PATH=/usr/include								--->	//GLOG_INCLUDE_DIR:PATH=/usr/include
																--->	GLOG_INCLUDE_DIR:PATH=/usr/local/include
//Path to a library.
GLOG_LIBRARY:FILEPATH=/usr/lib/x86_64-linux-gnu/libglog.so		--->	//GLOG_LIBRARY:FILEPATH=/usr/lib/x86_64-linux-gnu/libglog.so
																--->	GLOG_LIBRARY:FILEPATH=/usr/local/lib/libglog.so
//Folder contains Google glog
GLOG_ROOT_DIR:PATH=/home/lili/bruce/glog

//The directory containing a CMake configuration file for GFLAGS.
GFLAGS_DIR:PATH=/usr/local/lib/cmake/gflags

//Path to a file.
GFLAGS_INCLUDE_DIR:PATH=/usr/include							--->	//GFLAGS_INCLUDE_DIR:PATH=/usr/include
																--->	GFLAGS_INCLUDE_DIR:PATH=/usr/local/include
//Path to a library.
GFLAGS_LIBRARY:FILEPATH=/usr/lib/x86_64-linux-gnu/libgflags.so	--->	//GFLAGS_LIBRARY:FILEPATH=/usr/lib/x86_64-linux-gnu/libgflags.so
																--->	GFLAGS_LIBRARY:FILEPATH=/usr/local/lib/libgflags.so
//Folder contains Gflags
GFLAGS_ROOT_DIR:PATH=/home/lili/bruce/gflags

//Path to a file.
PROTOBUF_INCLUDE_DIR:PATH=/home/superuser/anaconda3/include								--->	//PROTOBUF_INCLUDE_DIR:PATH=/home/superuser/anaconda3/include
																						--->	PROTOBUF_INCLUDE_DIR:PATH=/usr/local/protobuf/include
//Path to a library.
PROTOBUF_LIBRARY:FILEPATH=/home/superuser/anaconda3/lib/libprotobuf.so					--->	//PROTOBUF_LIBRARY:FILEPATH=/home/superuser/anaconda3/lib/libprotobuf.so
																						--->	PROTOBUF_LIBRARY:FILEPATH=/usr/local/protobuf/lib/libprotobuf.so
//Path to a library.
PROTOBUF_LIBRARY_DEBUG:FILEPATH=/home/superuser/anaconda3/lib/libprotobuf.so			--->	//PROTOBUF_LIBRARY_DEBUG:FILEPATH=/home/superuser/anaconda3/lib/libprotobuf.so
																						--->	PROTOBUF_LIBRARY_DEBUG:FILEPATH=/usr/local/protobuf/lib/libprotobuf.so
//Path to a library.
PROTOBUF_LITE_LIBRARY:FILEPATH=/home/superuser/anaconda3/lib/libprotobuf-lite.so		--->	//PROTOBUF_LITE_LIBRARY:FILEPATH=/home/superuser/anaconda3/lib/libprotobuf-lite.so
																						--->	PROTOBUF_LITE_LIBRARY:FILEPATH=/usr/local/protobuf/lib/libprotobuf-lite.so
//Path to a library.
PROTOBUF_LITE_LIBRARY_DEBUG:FILEPATH=/home/superuser/anaconda3/lib/libprotobuf-lite.so	--->	//PROTOBUF_LITE_LIBRARY_DEBUG:FILEPATH=/home/superuser/anaconda3/lib/libprotobuf-lite.so
																						--->	PROTOBUF_LITE_LIBRARY_DEBUG:FILEPATH=/usr/local/protobuf/lib/libprotobuf-lite.so
//The Google Protocol Buffers Compiler
PROTOBUF_PROTOC_EXECUTABLE:FILEPATH=/home/superuser/anaconda3/bin/protoc				--->	//PROTOBUF_PROTOC_EXECUTABLE:FILEPATH=/home/superuser/anaconda3/bin/protoc
																						--->	PROTOBUF_PROTOC_EXECUTABLE:FILEPATH=/usr/local/protobuf/bin/protoc
//Path to a library.
PROTOBUF_PROTOC_LIBRARY:FILEPATH=/home/superuser/anaconda3/lib/libprotoc.so				--->	//PROTOBUF_PROTOC_LIBRARY:FILEPATH=/home/superuser/anaconda3/lib/libprotoc.so
																						--->	PROTOBUF_PROTOC_LIBRARY:FILEPATH=/usr/local/protobuf/lib/libprotoc.so
//Path to a library.
PROTOBUF_PROTOC_LIBRARY_DEBUG:FILEPATH=/home/superuser/anaconda3/lib/libprotoc.so		--->	//PROTOBUF_PROTOC_LIBRARY_DEBUG:FILEPATH=/home/superuser/anaconda3/lib/libprotoc.so
																						--->	PROTOBUF_PROTOC_LIBRARY_DEBUG:FILEPATH=/usr/local/protobuf/lib/libprotoc.so
//No help, variable specified on the command line.
PROTOBUF_ROOT_DIR:UNINITIALIZED=/home/lili/bruce/protobuf-3.6.0//5.1

//No help, variable specified on the command line.
PROTOC_ROOT_DIR:UNINITIALIZED=/home/lili/bruce/protobuf-3.6.0//5.1

```
```
(skip)$ cmake ..  -G "Unix Makefiles" \
(skip)    -DGLOG_ROOT_DIR=/home/lili/bruce/glog \
(skip)    -DGFLAGS_ROOT_DIR=/home/lili/bruce/gflags \
(skip)	-DCMAKE_CXX_COMPILER=g++-5
```

- [编译caffe时关于protobuf版本不同的问题](https://blog.csdn.net/m0_38082419/article/details/80117132)

刪除protobuf3.5.1安裝在/usr/local/protobuf/*的所有檔案，下protobuf3.6.0安裝，caffe make，成功  
```
$ sudo rm -rf /usr/local/protobuf
$ tar -xzvf protobuf-all-3.6.0.tar.gz
$ cd protobuf-3.6.0/
$ ./autogen.sh
$ ./configure --prefix=/usr/local/protobuf CC=/usr/bin/gcc
$ sudo make -j8
$ sudo make install
$ sudo ldconfig
$ cd python/
$ sudo python2.7 setup.py build
$ sudo python2.7 setup.py install
$ sudo python2.7 setup.py test
$ cd /home/lili/bnruce/github/NR-IQA-CNN/build
$ sudo cmake .. -DCMAKE_CXX_COMPILER=g++-5
$ sudo make all -j8
$ sudo make install
$ sudo make runtest 

```

### Caffe - No module named caffe
- Show Error
```
$ python2.7
Python 2.7.12 (default, Nov 12 2018, 14:36:49) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import caffe
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named caffe
>>> 
```
- Solve
```
$ vim ~/.bashrc
--->export PYTHONPATH=/home/lili/bruce/github/NR-IQA-CNN/python:$PYTHONPATH
$ source ~/.bashrc
```

### Caffe - No module named skimage.io
- Show Error
```
$ python2.7
Python 2.7.12 (default, Nov 12 2018, 14:36:49) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import caffe
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/lili/bruce/github/NR-IQA-CNN/python/caffe/__init__.py", line 1, in <module>
    from .pycaffe import Net, SGDSolver, NesterovSolver, AdaGradSolver, RMSPropSolver, AdaDeltaSolver, AdamSolver
  File "/home/lili/bruce/github/NR-IQA-CNN/python/caffe/pycaffe.py", line 15, in <module>
    import caffe.io
  File "/home/lili/bruce/github/NR-IQA-CNN/python/caffe/io.py", line 2, in <module>
    import skimage.io
ImportError: No module named skimage.io
>>> 

```
- Solve
```
$ sudo apt-get install python-matplotlib python-numpy python-pil python-scipy
$ sudo apt-get install build-essential cython
$ sudo apt-get install python-skimage
```
### Caffe - SyntaxError: invalid syntax
- Show Error
```
$ python2.7
Python 2.7.12 (default, Nov 12 2018, 14:36:49) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> improt caffe
  File "<stdin>", line 1
    improt caffe
               ^
SyntaxError: invalid syntax
>>>
```
- Solve
```
$ pip install python-dateutil --upgrade
```
### Caffe - Success Import
```
$ python2.7
Python 2.7.12 (default, Nov 12 2018, 14:36:49) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import caffe
/usr/lib/python2.7/dist-packages/matplotlib/font_manager.py:273: UserWarning: Matplotlib is building the font cache using fc-list. This may take a moment.
  warnings.warn('Matplotlib is building the font cache using fc-list. This may take a moment.')
>>>
```

### Matlab rum file

- Show Error
```
$ matlab -nodisplay -nodesktop -r prep_training_data

                                            < M A T L A B (R) >
                                  Copyright 1984-2016 The MathWorks, Inc.
                                   R2016b (9.1.0.441655) 64-bit (glnxa64)
                                             September 7, 2016

 
To get started, type one of these: helpwin, helpdesk, or demo.
For product information, visit www.mathworks.com.
 
Warning: uigetdir is no longer supported when MATLAB is started with the -nodisplay or -noFigureWindows option
or there is no display. For more information, see "Changes to -nodisplay and -noFigureWindows Startup Options"
in the MATLAB Release Notes. To view the release note in your system browser, run
web('www.mathworks.com/help/matlab/release-notes.html#br5ktrh-3', '-browser') 
> In warnfiguredialog (line 21)
  In uigetdir (line 56)
  In prep_training_data (line 17)
  In run (line 96) 
Error using javaObjectEDT
Scalar input must be a java object

Error in matlab.ui.internal.dialog.Dialog/getParentFrame (line 46)
               obj.ParentFrame = javaObjectEDT(com.mathworks.hg.peer.utils.DialogUtilities.createParentWindow);

Error in matlab.ui.internal.dialog.FileSystemChooser/getParentFrame (line 129)
                parframe = getParentFrame@matlab.ui.internal.dialog.Dialog(obj);

Error in matlab.ui.internal.dialog.FolderChooser/doShowDialog (line 70)
            javaMethodEDT('showOpenDialog', obj.Peer, getParentFrame(obj));

Error in matlab.ui.internal.dialog.FolderChooser/show (line 48)
            doShowDialog(obj)

Error in uigetdir_helper (line 32)
    dirdlg.show();

Error in uigetdir (line 57)
[directoryname] = uigetdir_helper(varargin{:});

Error in prep_training_data (line 17)
inputDir = uigetdir('.', 'Select input images'' directory');

Error in run (line 96)
evalin('caller', [script ';']);
 
Exception in thread "AWT-EventQueue-0" java.awt.HeadlessException
	at java.awt.GraphicsEnvironment.checkHeadless(Unknown Source)
	at java.awt.Window.<init>(Unknown Source)
	at java.awt.Frame.<init>(Unknown Source)
	at javax.swing.JFrame.<init>(Unknown Source)
	at com.mathworks.mwswing.MJFrame.<init>(MJFrame.java:108)
	at com.mathworks.mwswing.MJFrame.<init>(MJFrame.java:101)
	at com.mathworks.hg.peer.utils.DialogUtilities$1.runWithOutput(DialogUtilities.java:58)
	at com.mathworks.jmi.AWTUtilities$Invoker$2.watchedRun(AWTUtilities.java:475)
	at com.mathworks.jmi.AWTUtilities$WatchedRunnable.run(AWTUtilities.java:436)
	at java.awt.event.InvocationEvent.dispatch(Unknown Source)
	at java.awt.EventQueue.dispatchEventImpl(Unknown Source)
	at java.awt.EventQueue.access$200(Unknown Source)
	at java.awt.EventQueue$3.run(Unknown Source)
	at java.awt.EventQueue$3.run(Unknown Source)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.security.ProtectionDomain$1.doIntersectionPrivilege(Unknown Source)
	at java.awt.EventQueue.dispatchEvent(Unknown Source)
	at java.awt.EventDispatchThread.pumpOneEventForFilters(Unknown Source)
	at java.awt.EventDispatchThread.pumpEventsForFilter(Unknown Source)
	at java.awt.EventDispatchThread.pumpEventsForHierarchy(Unknown Source)
	at java.awt.EventDispatchThread.pumpEvents(Unknown Source)
	at java.awt.EventDispatchThread.pumpEvents(Unknown Source)
	at java.awt.EventDispatchThread.run(Unknown Source)
>>

```
- [Error using javaObjectEDT Scalar input must be a java object](https://github.com/icaoberg/docker-cellorganizer-jupyter-notebook/issues/12)  

uigetdir會開一個ui讓你選資料夾  
server沒UI，直接改程式碼  
```
%inputDir = uigetdir('.', 'Select input images'' directory');
%inputDir = strcat(inputDir, '/');
inputDir = './databaserelease1/';
```
### Other Command
```
$ 7za a -t7z -mx=9 databaserelease1-train.7z databaserelease1
$ 7z x databaserelease1.7z
```

- [图像质量评估综述](https://zhuanlan.zhihu.com/p/32553977)


质量评估可分为  
图像质量评估(Image Quality Assessment, IQA)  
视频质量评估(Video Quality Assessment, VQA)  

IQA可分为主观评估和客观评估  

主观评估一般采用  
平均主观得分(Mean Opinion Score, MOS)  
平均主观得分差异(Differential Mean Opinion Score, DMOS)  

客观评估使用数学模型给出量化值  
使用图像处理技术生成一批失真图像  

IQA按照原始参考图像提供信息分成3类  
全参考(Full Reference-IQA, FR-IQA)  
半参考(Reduced Reference-IQA, RR-IQA)  
无参考(No Reference-IQA, NR-IQA)盲参考(Blind IQA, BIQA)  

原始(无失真、参考)图像和失真图像  
只有失真图像  
