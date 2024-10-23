
# Falcon Missile Simulation

This is a fork of threeD repository from gedeschaines. Unlike the original project this project uses C++ instead of C and OpenGL instead of X11. This program which displays animated 3D faceted solid shape rendering of missile/target engagements recorded in trajectory files output from 3-DOF and 6-DOF surface-to-air missile (SAM) flyout simulations. 


## Repository Structure

The repository main directory contains this README file, plain text disclaimer file and the following script files:

Exec_falcon - Bash script to execute ./bin/falcon.exe for specified TXYZ.OUT case number
Make_falcon - Bash script to invoke GNU make with Makefile appropriate for identified system
Makefile_Linux - Makefile used by GNU make to build falcon excutable on Linux system
Makefile_Cygwin - Makefile used by GNU make to build falcon excutable on Cygwin system
txyz_symlink - VS Code debugging preLaunch script to create TXYZ.OUT symbolic link

he contents of each subdirectory are as follows:

.vscode - MS Visual Studio Code workspace task and launch settings JSON files.
bin - falcon program executable (Exists only in local repository workspace)
dat - Facet model polygon data files for missile, target, ground grid and plane.
doc - Usage and informational documentation
src - C source code files comprising the falcon program
txyz - TXZY.OUT trajectory data files for sample missile/target engagement cases
util - Bash scripts to merge captured image sequences into animated GIF or MP4 video files
Ximg - Images captured during falcon execution (Exists only in local repository workspace)

The provided shell scripts enable a user to build and execute falcon entirely independent of MS Visual Studio Code IDE. However, the .vscode subdirectory contains build task and debug launch settings JSON files for those inclined to work with falcon using MS VS Code. This must be done with VS Code running a WSL2 installed Linux distribution, and with appropriate C/C++ extensions for editing, analyzing, compiling and debugging C code.

Latest development efforts for the falcon program have been on bootable Ubuntu 14.04, 16.04 and 18.04 Linux systems, and on a Windows 11 system with WSL2 running an Ubuntu 20.04 Linux distribution. In the past, falcon had been successfully built and executed with Cygwin on Windows 2000, XP and 7 systems, and recently the provided Makefile_Cygwin file was used to successfully build falcon with Cygwin DLL version 3.4.9 on a Windows 11 version 22H2 system. In addition to GNU make and gcc toolchains, the following libraries and applications are required to build and execute falcon, and work with captured pixel image files.

libX11-dev for X11 client-side library libX11 and include headers
libxt-dev for X11 toolkit intrinsics libXt and include headers
libxpm-dev for X11 pixmap libXpm and include headers
libmotif-dev for Motif libXm and include headers
ImageMagick for convert app to convert XPM to GIF/JPEG formats and create animated GIF
ffmpeg to create MP4 video file from JPEG image sequence
ffplay or other video player such as MS Windows Media Player or VLC media player.

The falcon program is an X11 application that opens a desktop window in which animated 3D rendering is displayed. On Linux and Windows systems, a running X server is required. Most Linux distributions provide and start X server prior to displaying the desktop graphical interface. On Windows a third party X server application, such as those provided by VcXsrv or Cygwin, must be installed and activated before falcon can run. The following web articles provide information to get VcXsrv installed and working with a Windows 10 WSL2 Linux distribution.

WSL2 GUI X-Server Using VcXsrv
X11 - Guide to WSL

If needing to use ssh or rdp to connect to a running WSL Linux instance, having the following /etc/wsl.conf file on the installed Linux distribution is convenient.

[boot]
#systemd=true
command="service dbus start; service ssh start; service xrdp start"


## Execution Overview

The following steps delineate typical falcon utilization from a bash shell terminal.

1. Build falcon executable
From within the ./falcon directory, invoke ./Make_falcon to build "./bin/falcon.exe".

  This step only needs to be performed when the falcon repository is first installed, and after any code changes have been made to falcon source files.

2. Render animation and capture image sequence
  From within the ./falcon directory, invoke ./Exec_falcon with a specified TXYZ.OUT case number (i.e., 0000, 0001, 0002 or 0003).

  A blank falcon display window should appear on the desktop and keypress options to control the animation printed in the terminal window. Animation starts by clicking a mouse button when the mouse cursor is placed within the falcon display window. Keypresses are only   effective if the mouse cursor is within the display window. Pressing the "Esc" key or closing the falcon display window will terminate the program.

3. Create animated GIF or MP4 video
  Upon termination of the falcon program, change to the ./Ximg subdirectory and invoke ../util/xpm2gif to create animated GIF file named "img_anim.gif", or invoke ../util/xpm2mp4 to create MP4 video file named "img_anim.mp4".

  Note, to prevent captured and converted images from a previous falcon execution being incorporated into a subsequent animated GIF or MP4 video file, it's recommended all img_####.[xpm|gif|jpg] files be deleted from the ./Ximg subdirectory before beginning another       falcon execution. Also, rename or move the newly created animated GIF or MP4 video file to prevent it being overwritten.




## Caveats

All input facet model shape polygon data file paths are hard coded in the draw3D.c file. This was done to shift the task of assigning file paths to OS shell scripts external to threeD for batch processing management. Although the program is now interactive, batch processing use cases were not human-in-the-loop.

Quadrilateral polygons are used for shape facets, instead of triangles as in modern surface mesh models, since it was easier and faster to decompose missile and target shapes, and ground plane grids into quad patches. Also, there was no requirement to calculate accurate surface normals to model realistic lighting effects or perform face culling. Each facet is assigned a constant fill color, and considered visible on both surfaces. Consistency in direction around perimeter of a shape model facet that polygon vertice are specified should be maintained to facilitate possibilty of face culling being introduced in future rendering enhancements. Currently, the order adheres to right-hand-rule such that the positive normal is in a facet's +Z direction.

There is no attempt to resolve problems rendering intersecting polygons. Other than time of intercept, as missile impacts target, adjoined polygons do not intersect others.

The provided TXYZ.OUT.000# trajectory files generated by a Mathcad 3-DOF kinematic model of an ideal (no control lag with 100% effective, but bounded commanded acceleration and perfect command response) pure proportional navigation (N=4) guided missile correspond to the following engagement scenarios for a typical MANPADS type SAM.

0000 - Fixed-wing target at constant speed, altitude and heading; SAM launched with 12 degree elevation and 10 degree lead azimuth.
0001 - Fixed-wing target performing a constant 2g banking level turn toward its left; SAM launched with 12 degree elevation and 10 degree lead azimuth.
0002 - Fixed-wing target performing a constant 2g banking descending turn toward its left; SAM launched with 12.6 degree elevation and 27 degree lead azimuth.
0003 - Fixed-wing target performing a constant 3g banking descending turn toward its left; SAM launched with 14 degree elevation and 27.2 degree lead azimuth.
In each case the SAM, with constant velocity of 450 meters/second, was launched against the target flying at 200 meters/second, initially located at 2000 meters downrange, 500 meters height above ground level, and heading 90 degrees left of the missile launch location. For a sense of relative dimensions of missile and target, the fixed-wing target shape model is scaled to about the size of a Piper M350 aircraft.


## References

[1] David F. Rogers,"Procedural Elements for Computer Graphics" McGraw-Hill, Inc., 1985.

[2] Robert Sedgewick and Keven Wayne, "Algorithms, 4th edition", Addison Wesley Professional, March 23, 2011. Web available at algs4.cs.princeton.edu
## Disclaimer

This project is a refactored version of the original threeD project, converted from C to C++ and integrated with OpenGL for rendering. The following points should be noted:

Experimental Nature: This project is an experimental adaptation of the original code and may contain modifications that differ from the initial design. The code is provided as-is, and there is no guarantee of full functionality or performance equivalence to the original implementation.

Platform-Specific Behavior: While this project is designed to work with OpenGL, compatibility with different systems and devices (such as STM32MP157 or other embedded platforms) may vary. Proper OpenGL ES support and adjustments for embedded platforms are necessary, and users are responsible for ensuring compatibility with their target hardware.

Third-Party Libraries: This project may require external libraries such as GLFW or GLEW to handle OpenGL context creation and management. Users are responsible for installing and configuring these dependencies.

Limited Support: No official support is provided for this refactored version, and it is intended for educational and research purposes. Contributions and modifications are welcome, but they are not guaranteed to be merged into the repository.

Licensing: The project continues to follow the licensing terms of the original threeD repository. Users must comply with the licensing requirements specified in the original codebase.

