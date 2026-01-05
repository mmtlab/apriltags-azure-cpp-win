apriltags-cpp(Windows)
=============

C++ port of the APRIL tags library, using OpenCV (and optionally, CGAL).


Update
============
P-Chao 2018 
* 20181123 build apriltags-cpp for windows


Requirements
============

Currently, apriltags-cpp requires the following to build:

  * OpenCV >= 2.3

You must have cmake installed to build the software as well.

Building
========

To compile the code, 

    cmake -Bbuild
    cmake --build build --config Release

Usage Examples
==============

To see available options:

    ./build/Release/camera_apriltags.exe -h

Example usage with Azure Kinect recording:

    ./build/Release/camera_apriltags.exe -c "azure" -i "path/to/recording.mkv" -o "path/to/output"



