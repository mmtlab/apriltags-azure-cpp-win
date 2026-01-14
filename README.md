APRILTAGS C++ (Windows & Azure Kinect)
=======================

C++ port of the APRIL Tags library using OpenCV, with optional Azure Kinect support. This Windows-focused fork builds a static library (`apriltags`) and a demo tool (`camera_apriltags`).

Overview
--------
- `apriltags`: Core library for AprilTag detection.
- `camera_apriltags`: Demo tool for live camera or recording files, supporting both OpenCV cameras and Azure Kinect.

Requirements
------------
- CMake ≥ 3.5
- C++17-capable compiler (MSVC recommended on Windows)
- OpenCV (Windows builds require `OpenCV_DIR` to be set)
- Optional: Azure Kinect Sensor SDK (`k4a`) and Recording SDK (`k4arecord`)

Notes for Windows
-----------------
- Set the `OpenCV_DIR` environment variable to your OpenCV installation's build folder, e.g. `C:\opencv\build`.
- Azure Kinect is auto-detected in common install paths under `C:\Program Files`. You can also set `K4A_DIR` or `KINECT_AZURE_SDK` if using custom locations.
- You can disable Azure Kinect support at configure time with `-DSKIP_KINECT_AZURE=ON`.

Build (Windows, Visual Studio)
------------------------------
Prerequisites:
- Install OpenCV for Windows: https://opencv.org/releases/ (extract to `C:\opencv`).
- Install CMake for Windows: https://cmake.org/download/
- Optional: Install Azure Kinect Sensor SDK v1.4.x if you plan to use `-c azure`: https://www.microsoft.com/en-us/download/details.aspx?id=101454

Environment setup (PowerShell):
```powershell
# Set OpenCV location (adjust if different)
setx OpenCV_DIR "C:\opencv\build" /M

# Optional: Azure Kinect (adjust version/path as needed)
setx K4A_DIR "C:\Program Files\Azure Kinect SDK v1.4.1" /M
$env:Path = "$env:Path;C:\Program Files\Azure Kinect SDK v1.4.1\tools"
```

Configure and build:
```powershell
# From the repo root
cmake -S . -B build
cmake --build build --config Release
```

Notes:
- To disable Azure Kinect entirely: add `-DSKIP_KINECT_AZURE=ON` to the configure step.

Usage
-----
Run `camera_apriltags` and use `-h` to list options.

Examples:
- OpenCV webcam (device 0):
```powershell
build\Release\camera_apriltags.exe -c opencv -d 0 -f Tag36h11 -z 0.22
```
- OpenCV recording (MP4, AVI, etc.):
```powershell
build\Release\camera_apriltags.exe -c opencv -i "C:\path\to\video.mp4"
```
- Azure Kinect live camera:
```powershell
build\Release\camera_apriltags.exe -c azure
```
- Azure Kinect MKV playback:
```powershell
build\Release\camera_apriltags.exe -c azure -i "C:\path\to\recording.mkv"
```

Saving outputs
--------------
- Output directory: `-o DIR` sets where files are written; default is `images` (relative to the working directory). The directory is created if it does not exist.
- Filenames: `<base>_color.png` and `<base>_corners.json` (corners for all detected tags). `base` is the camera serial or input file name for Azure, and `camera` or the input file name for OpenCV.
- Point cloud: for Azure Kinect, pressing `s` stages the current frame and point cloud to be saved as `<base>_point_cloud.ply` on exit. If you skip `s`, the PLY is not written. For OpenCV cameras, no point cloud is produced.
- Save trigger: press `s` during capture to choose which frame/point cloud to save; files are written when the program exits (e.g., after pressing `Esc`).

Common options:
- `-f FAMILY`: Tag family (e.g., `Tag36h11`).
- `-z SIZE`: Tag size in meters.
- `-W WIDTH -H HEIGHT`: Frame size (OpenCV camera).
- `-M`: Toggle display mirroring.
- `-o DIR`: Output directory for saved images.



Azure Kinect Setup
------------------
- Install the Azure Kinect Sensor SDK (v1.4.x recommended).
- Ensure SDK libraries and headers are in standard locations under `Program Files`, or set `K4A_DIR` / `KINECT_AZURE_SDK` environment variables.
- If Azure Kinect SDK is not available, use `-c opencv` or configure with `-DSKIP_KINECT_AZURE=ON`.

Project Structure
-----------------
- Core sources in `src/` (e.g., `TagDetector.cpp`, `TagFamily.cpp`).
- Demo sources: `camera_apriltags.cpp`; Azure helpers in `azure_kinect_helpers.*`.
- CMake find modules: `Findk4a.cmake`, `Findk4arecord.cmake`.

Acknowledgments
---------------
- Original APRIL Tags work by Edwin Olson and contributors.
- C++ port and modifications by Matt Zucker and others.
- Windows fork and repository by P-Chao: https://github.com/P-Chao/apriltags-cpp-win
