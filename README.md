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
---
## Matlab Install

- [ubuntu16.04&matlab2016b](https://blog.csdn.net/Jesse_Mx/article/details/53956358)
- [ubuntu-non-UI&matlab2016b](https://blog.csdn.net/zhaohuihuihi/article/details/77455715)
- [ubuntu-non-UI&matlab2017b](https://blog.csdn.net/Xiao_Song_PKU/article/details/82700228)
- mount 1st iso file
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
- (skip)activate again for safety
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
	gcc和g++對齊到version 5.4.0 20160609									->失敗	undefined reference to `google::protobuf
	下了protobuf3.5.1，用源碼安裝再make									->失敗	undefined reference to `google::protobuf
	接著下了glog和gflags，用源碼安裝再make									->失敗	undefined reference to `google::protobuf
	遇到gflags源碼安裝的其他問題，修改common.hpp問題消失但make還是		->失敗	undefined reference to `google::protobuf
	發現caffe cmake時，glog、gflags、protbuf的路徑無法被修改
	接著改~/caffe/build/CMakeCache.txt的glog、gflags、protbuf的路徑
	路徑成功修改但make還是													->失敗	undefined reference to `google::protobuf
	
	最後查看server上不只3.5.1版本，在另一個anaconda上也有3.5.1和3.4.0
	有人說不同版本共存會有衝突，將不同版本的protbuf刪除再編譯
	礙於多人共同使用server，我沒有這樣做
	正要考慮直接用anaconda創一個新環境來搞
	最後在放棄之前下了protobuf3.6.0及3.3.0
	先刪除3.5.1，$ sudo rm -rf /usr/local/protobuf，接著裝3.6.0，make		->幹終於成功了
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






```