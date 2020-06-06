 



OpenCV is a very powerful and complicated, multi-language (C, C++ and Python) library with many dependencies, versions and compilation flags.  This page was created to help identify package dependencies and key cmake parameters that impact algorithm real-time performance.  Furthermore, the OpenCV.org installation instructions (Ref 1) aren't very thorough (IMO) and are incomplete (e.g. missing definition of many key cmake parameters like WITH_CUDA) which is another reason this page was created.  If using Python or real-time performance isn't critical, different dependencies and compilation flag values may be desired. 

Update/Upgrade Package Manager
sudo apt update
sudo apt upgrade
Install Dependencies
# save the current working directory
cwd=$(pwd)

########################################################################
## Install required dependencies
# would like to add comment to all identifying why each dependency is required
# Note: provide -y to provide yes automatically to prompts (e.g. disk space requested for package)

sudo apt-get install build-essential					# contains native compiler
sudo apt-get install git								# required to clone opencv git repositories
sudo apt-get install cmake								# OpenCV uses cmake for build/install
sudo apt-get install pkg-config 						# 

# These packages are required for GUI features, camera streams and video processing
sudo apt-get install libavcodec-dev 
sudo apt-get install libavformat-dev					# general framework for multiplexing and demultiplexing audio, 
														# video and subtitle streams. It also supports several input 
														# and output protocols to access media resources
sudo apt-get install libswscale-dev
sudo apt-get install libgstreamer0.10-dev 				# streaming media framework based on graphs of filters which 
														# operate on media data. You can perform real-time sound processing 
														# to playing videos with this library. It has plugin-based architecture 
														# which means that new processing capabilities can be added simply
sudo apt-get install libgstreamer-plugins-base0.10-dev	# package contains development files for GStreamer libraries from 
														# the ‘base’ set, an essential exemplary set of elements
sudo apt-get install libgtk2.0-dev						# free and open source cross-platform widget toolkit for creating GUIs

# These are optional according to [1]. OpenCV comes with supporting files for image formats like 
# PNG, JPEG, JPEG2000, TIFF, WebP etc but the versions may be old. If you want to ensure you have the latest, try installing them
sudo apt-get install libpng-dev							# used to handle PNG format images
sudo apt-get install libjpeg-dev						# This is a free library for handling JPEG Image Data format. It has 
														# got various utilities such as cjpeg and djpeg to convert the JPEG 
														# data into other image file formats. It also has rdjpgcom and 
														# wrjpgcom which helps to insert and extract textual comments in 
														# JPEG files.
sudo apt-get install libopenexr-dev
sudo apt-get install libtiff-dev						# for reading and writing Tagged Image File Format (TIFF) files
sudo apt-get install libwebp-dev						#
sudo apt-get install libxine2-dev						# library provides complete infrastructure for a media player. The 
														# format supported are MPEG 1/2, some AVI and Quicktime videos. It 
														# also supports network streams, subtitles and even MP3 or Ogg files
sudo apt-get install libjasper-dev						# for JPEG2000 Part-1 support on Ubuntu 16.04
sudo apt-get install libdc1394-22-dev					# library allows a program to interface with camera which 
														# conforms to IEEE 1394 standard

# These are optional according to [1] but probably depend on cmake parameter values (see comments)
sudo apt-get install libv4l-dev							# for V4L camera support, required if cmake parameter WITH_V4L is ON and BUILD_TBB is OFF
sudo apt-get install libtbb2							# for Intel TBB support, required if cmake parameter WITH_TBB is ON and BUILD_TBB is OFF
sudo apt-get install libtbb-dev 						# if using TBB for development ... TODO: see if this is used for ARM64
sudo apt-get install qt5-default						# required if cmake parameter WITH_QT is ON? cross-platform C++ application 
														# framework. It has a rich set of widgets which provides standard GUI 
														# functionality. It is classified as widget toolkit
sudo apt-get install libeigen3-dev 						# lightweight C++ template library for vector and matrix math, a.k.a. Linear Algebra. 
														# It is dedicated to provide optimal speed with GCC, required if WITH_EIGEN is ON
########################################################################
# the following packages were implied dependency/recommendations by [2] but not mentioned by [1]
#
sudo apt-get install yasm								# This is an assembler which supports x86 and AMD64 instruction 
														# sets; TBD why its recommended
sudo apt-get install checkinstall 						# program creates native package for your linux distribution and enabling  
														# software to be installed as by your distribution package management system (dpkg, rpm)
sudo apt-get install libjpeg8-dev						#  
sudo apt-get install libpng12-dev						# 
sudo apt-get install libtiff5-dev						# 
sudo apt-get install libxine2-dev 						# 
sudo apt-get install libfaac-dev 						# library is AAC encoder and FAAD2 decoder. It supports several MPEG-4 
														# object types (LC, Main, LTP, HE AAC, PS) and file formats (ADTS AAC, raw AAC, MP4)
sudo apt-get install libmp3lame-dev 					# MP3 encoding library which is used to for sound analysis as well as convenience tools
sudo apt-get install libtheora-dev						# Theora Video Compression Codec. Theora is a fully open, non-proprietary, 
														# general purpose compressed video format
sudo apt-get install libvorbis-dev 						# library contains development files for Vorbis General Audio Compression Codec. 
														# Ogg Vorbis is a fully open, non-proprietary, general purpose compressed audio 
														# format for audio and music at fixed and variable bitrates from 16 to 128 kbps/channel
sudo apt-get install libxvidcore-dev					# library contains development files for MPEG-4 video codec. Xvid is an open source MPEG-4 
														# video codec, implementing MPEG-4 Simple Profile, Advanced Simple Profile and Advanced 
														# Video Coding standards. It is written in C with assembler optimizations for quality and speed
sudo apt-get install libopencore-amrnb-dev				# library contains an implementation of the 3GPP TS 26.073 specification for the Adaptive 
														# Multi Rate (AMR) speech codec. The implementation is derived from the OpenCORE framework, 
														# part of the Google Android project
sudo apt-get install libopencore-amrwb-dev				# library contains an implementation of the 3GPP TS 26.173 specification for the Adaptive 
														# Multi Rate – Wideband (AMR-WB) speech decoder. The implementation is derived from the 
														# OpenCORE framework part of the Google Android project
sudo apt-get install libavresample-dev					#
sudo apt-get remove x264								# deleting package first before re-install was recommended by [2]
	
sudo apt-get install x264 								# free and open-source software library and a command line utility developed by VideoLAN 
														# for encoding video streams into the MPEG-4 AVC format. It is a very high quality encoder 
														# and produces remarkable quality in bitstreams
sudo apt-get libx264-dev								# 
sudo apt-get install v4l-utils							# collection of command line video4linux utilities such as decode_tm6000 which decodes 
														# tm6000 proprietary format streams. It also has a utility which can receive and 
														# decode Radio Data System (RDS) streams
sudo apt-get install libprotobuf-dev 					# library is responsible for serializing structured data which is similar to XML. 
														# This will help to easily write and read your structured data to and from a variety 
														# of data streams
sudo apt-get install protobuf-compiler					# package contains the protocol buffer compiler that is used for translating from .proto 
														# files (containing the definitions) to the language binding for the supported languages
sudo apt-get install libgoogle-glog-dev 				# library provides logging APIs based on C++ style streams and helper macros
sudo apt-get install libgflags-dev						# library implements command line flags processing. It gives you the ability to define 
														# flags in the source file
sudo apt-get install libgphoto2-dev 					# library can be used by applications to access various digital camera models, via standard 
														# protocols such as USB Mass Storage and PTP, or vendor-specific protocols
sudo apt-get install libhdf5-dev 						# library contains development files for Hierarchical Data Format 5 (HDF5). HDF5 is designed 
														# to store and organize large amounts of data
sudo apt-get install doxygen							# tool is used to generate an online documentation for annotated C++ sources

# these are mentioned by [2] and [3] states they help optimize some OpenCV functions:
sudo apt-get install gfortran							# compile Fortran programs to get the desired output during execution. The GFortran will 
														# develop the Fortran compiler front end and run-time libraries for GCC

sudo apt-get install libatlas-base-dev					# library supplies optimized versions for the complete set of linear 
														# algebra kernels known as Basic Linear Algebra Subroutines (BLAS) 
														# and a subset of the linear algebra routines in the LAPACK library. 
														# ATLAS is an acronym for Automatically Tuned Linear Algebra Software

# TBD why this is recommneded by [2]
cd /usr/include/linux
sudo ln -s -f ../libv4l1-videodev.h videodev.h
cd $cwd
Get Sources
cd $cwd

#Specify OpenCV version
OPENCV_VERSION=3.4.10
git clone -b ${OPENCV_VERSION} https://github.com/opencv/opencv.git

# this step is optional; only required if you'd
# like to use the 3rd party/contributor library
# Note: check licensing of anything in this library before using
git clone -b ${OPENCV_VERSION} https://github.com/opencv/opencv_contrib.git

# create build directory
cd opencv
mkdir build
cd build
 Generate Makefile
The following is the recommended flags for building OpenCV for an embedded target.  Notice the OPENCV_ENABLE_NONFREE flag is set to OFF; this is a safe-guard intended to prevent non-free software from being deployed on AV products.  If building the libraries for a PC, you may want to change the build and install flags for the examples and tests to ON.  If running OpenCV on an ARM-based Linux system, set the ENABLE_VFP3, ENABLE_NEON, WITH_TBB and BUILD_TBB to ON; the compiler doesn't enable these by default and will result in a 30% speed increase[12].

# generate makefile using cmake
# space is required
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
	-D OPENCV_EXTRA_MODULES_PATH=$cwd/opencv_contrib/modules \
	-D OPENCV_ENABLE_NONFREE=OFF \
	-D BUILD_TEST=OFF \
	-D BUILD_PERF_TEST=OFF \
    -D BUILD_EXAMPLES=OFF \
	-D BUILD_TBB=OFF \
	-D INSTALL_TESTS=OFF \
	-D INSTALL_C_EXAMPLES=OFF \
	-D WITH_CUDA=ON \
    -D WITH_TBB=ON \
	-D WITH_OPENMP=ON \
	-D WITH_IPP=ON \
	-D WITH_NVCUVID=ON \
    -D WITH_V4L=ON \
    -D WITH_QT=ON \
    -D WITH_OPENGL=ON \
	-D WITH_OPENCL=ON \
	-D WITH_LAPACK=OFF \
	-D WITH_EIGEN=ON \
	-D ENABLE_NEON=OFF \
	-D ENABLE_VFPV3=OFF \
	-D WITH_CSTRIPES=OFF ..


############################################
# Description of cmake options
# CMAKE_BUILD_TYPE				-->	build type, can be RELEASE (optimized) or DEBUG (optimization off, w/ debug symbols)
# CMAKE_INSTALL_PREFIX			-->	this is where cmake will install the generate library files (i.e. outcome of 'sudo make install')
# CMAKE_SHARED_LINKER_FLAGS		--> '-latomic'
# ENABLE_PROFILING				--> Enable profiling in the GCC compiler (Add flags: -g -pg)
# OPENCV_EXTRA_MODULES_PATH		-->	need to specify location of opencv_contrib/modules in order for them to be built by cmake
# OPENCV_ENABLE_NONFREE			--> Enable non-free algorithms
# OPENCV_FORCE_3RDPARTY_BUILD	--> Force using 3rd party code from source

# Optional 3rd party components
# there are more options, these are just the ones that seemed most interesting / performance impact
# ===================================================
# WITH_OPENMP					--> Include OpenMP support
# WITH_IPP						--> Include Intel IPP support
# WITH_NVCUVID					-->
# WITH_CUDA						-->
# WITH_TBB						-->	Include Intel Threading Building Blocks (TBB) support
# WITH_V4L						-->	Include Video 4 Linux support
# WITH_QT						-->	Build with Qt Backend support
# WITH_OPENGL					-->	OpenGL support for building application and viewing images/videos
# WITH_OPENCL					--> Include OpenCL Runtime support (NOT CV_DISABLE_OPTIMIZATION)
# WITH_GSTREAMER				--> Include Gstreamer support
# WITH_GSTREAMER_0_10			--> Enable Gstreamer 0.10 support (instead of 1.x)
# WITH_OPENCLAMDBLAS 			--> Include AMD OpenCL BLAS library support
# WITH_LIBREALSENSE				--> Include Intel librealsense support

# WITH_CAROTENE					--> Use NVidia carotene acceleration library for ARM platform
# WITH_CUBLAS					--> Include NVidia Cuda Basic Linear Algebra Subprograms (BLAS) library support
# WITH_FFMPEG					--> Include FFMPEG support
# WITH_EIGEN					--> Include Eigen2/Eigen3 support
# WITH_GTK						--> Include GTK support
# WITH_GTK_2_X					--> Use GTK version 2
# WITH_PROTOBUF					--> Enable libprotobuf
# WITH_CSTRIPES					--> Include C= support
# WITH_GDAL						--> Include GDAL Support
# WITH_LAPACK					--> Include Lapack library support (NOT CV_DISABLE_OPTIMIZATION)


# OpenCV build components
# ===================================================
# BUILD_SHARED_LIBS				--> Build shared libraries (.dll/.so) instead of static ones (.lib/.a)
# BUILD_EXAMPLES				-->	Build all examples
# BUILD_DOCS					--> Create build rules for OpenCV Documentation
# BUILD_TEST					--> Build accuracy & regression tests
# BUILD_PERF_TEST				--> Build performance tests
# BUILD_WITH_DEBUG_INFO			--> Include debug info into release binaries


# OpenCV installation options
# ===================================================
# INSTALL_CREATE_DISTRIB		--> Change install rules to build the distribution package
# INSTALL_C_EXAMPLES			-->	Examples are saved in the sample folder which is present in 
# INSTALL_PYTHON_EXAMPLES		-->	the extracted folder of OpenCV
# INSTALL_TESTS					--> Install accuracy and performance test binaries and test data

# OpenCV build options
# ===================================================
# ENABLE_COVERAGE				--> Enable coverage collection with  GCov
# ENABLE_NEON					--> Enable NEON instructions
# ENABLE_VFPV3					--> Enable VFPv3-D32 instructions
# ENABLE_FAST_MATH				--> Enable compiler options for fast math optimizations on FP computations (not recommended)
# ENABLE_NOISY_WARNINGS			--> Show all warnings even if they are too noisy
# OPENCV_WARNINGS_ARE_ERRORS	--> Treat warnings as errors
# CV_ENABLE_INTRINSICS			--> Use intrinsic-based optimized code
# CV_DISABLE_OPTIMIZATION		--> Disable explicit optimized code (dispatched code/intrinsics/loop unrolling/etc)


# you can verify cmake parameter values using the following
cmake -LAH
Build and Install
# build libraries
# you can use nproc to find number of cores and increase 4 to nproc if you'd like
make -j4

# install libraries, headers, etc to CMAKE_INSTALL_PREFIX
sudo make install
References
https://docs.opencv.org/3.4.10/d7/d9f/tutorial_linux_install.html
https://www.learnopencv.com/install-opencv3-on-ubuntu/
https://www.pyimagesearch.com/2018/05/28/ubuntu-18-04-how-to-install-opencv/
https://docs.opencv.org/3.4.10/d2/de6/tutorial_py_setup_in_ubuntu.html
https://askubuntu.com/questions/1152639/how-to-obtain-the-same-performance-of-self-compiled-opencv-as-the-package-from-t
https://www.pyimagesearch.com/2017/10/09/optimizing-opencv-on-the-raspberry-pi/
https://www.theimpossiblecode.com/blog/build-faster-opencv-raspberry-pi3/
https://www.theimpossiblecode.com/blog/intel-tbb-on-raspberry-pi/
https://www.theimpossiblecode.com/blog/faster-opencv-smiles-tbb/
https://cv-tricks.com/installation/opencv-4-1-ubuntu18-04/
http://amritamaz.net/blog/opencv-config
https://docs.opencv.org/3.4.10/d0/d76/tutorial_arm_crosscompile_with_cmake.html
https://docs.opencv.org/master/df/d65/tutorial_table_of_content_introduction.html
 
