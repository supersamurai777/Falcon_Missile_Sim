# Falcon Missile Simulation #
This is a fork of threeD repository from gedeschaines. Unlike the original project this project uses C++ instead of C and OpenGL instead of X11. This program which displays animated 3D faceted solid shape rendering of missile/target engagements recorded in trajectory files output from 3-DOF and 6-DOF surface-to-air missile (SAM) flyout simulations.

## Background ##

The genesis of **threeD** began around 1986 as a Pascal program to perform 3D rendering of solid shapes on an Apple Mac, which only had 2D, 1-bit graphics capability. The progam was developed solely to learn and apply hidden surface removal algorithms before the advent of readily available 3D graphics libraries such as IRIS GL, Direct3D and OpenGL. At that time, most raster graphics hardware could perform 2D polygon drawing and filling, but multi-buffer (vertex, depth, frame, etc.) rendering pipelines were only available on expensive systems with proprietary graphics hardware, drivers and libraries, such as those developed by Intergraph, SGI and Tektronix. Most 3D rendering was performed in software, thus extremely CPU and memory intensive, and very slow; not at all conducive for realtime 3D rendering and animation.

Around the early 1990's, being employed at a weapon systems analysis firm involved in engineering analyses and simulation for SAM early warning and countermeasures projects provided an incentive to convert the Pascal program to FORTRAN and tailor it to process flight path and trajectory output files from several air combat and 3-DOF/6-DOF missile flyout simulations. It was primarily used as a post-processimg visualization tool to assist in simulation enhancement and V&V activities, and to generate presentation material illustrating areas of interest and concern. During this period the program evolved into several variants for different computing systems and graphics libraries.

By late 1990's and early 2000's, having resigned from the weapon systems analysis firm, continued work with the program languished. It became a novelty item to install on various personal Linux and Windows/Cygwin systems to evaluate X11 features amd capabilites, and resolve compatiblity issues. Recently it's been installed and executed on WSL2 Ubuntu 20.04 distribution for MS Windows 10 Pro version 22H2 (OS Build 19045.3570) running VcXsrv X Server version 1.20.9.0 (3 Jan 2021). Documented herein are the results of this effort.

## Repository Structure ##

The repository main directory contains this README file, plain text disclaimer file and the following script files:

+ **Exec_falcon** - Bash script to execute ./bin/falcon.exe for specified TXYZ.OUT case number
+ **Make_falcon** - Bash script to invoke GNU make with Makefile appropriate for identified system
+ **Makefile_Linux** - Makefile used by GNU make to build **falcon** excutable on Linux system
+ **Makefile_Cygwin** - Makefile used by GNU make to build **falcon** executable on Cygwin system
+ **txyz_symlink** - VS Code debugging pre-launch script to create TXYZ.OUT symbolic link

The contents of each subdirectory are as follows:

+ .vscode - MS Visual Studio Code workspace task and launch settings JSON files.
+ bin - **falcon** program executable **(Exists only in local repository workspace)**
+ dat - Facet model polygon data files for missile, target, ground grid and plane.  
+ doc - Usage and informational documentation (e.g., [Discord_Post_1](./doc/Discord_post_1.md))
+ src - C source code files comprising the **falcon** program
+ txyz - TXZY.OUT trajectory data files for sample missile/target engagement cases
+ util - Bash scripts to merge captured image sequences into animated GIF or MP4 video files
+ Ximg - Images captured during **falcon** execution **(Exists only in local repository workspace)**

The provided shell scripts enable a user to build and execute **falcon** entirely independent of MS Visual Studio Code IDE. However, the .vscode subdirectory contains build task and debug launch settings JSON files for those inclined to work with **falcon** using MS VS Code. This must be done with VS Code running a WSL2 installed Linux distribution, and with appropriate C/C++ extensions for editing, analyzing, compiling and debugging C code.

## Execution Prerequisites ##

Latest development efforts for the **falcon** program have been on bootable Ubuntu 14.04, 16.04 and 18.04 Linux systems, and on a Windows 10 Pro system with WSL2 running an Ubuntu 20.04 Linux distribution. In the past, **falcon** had been successfully built and executed with Cygwin on Windows 2000, XP and 7 systems, and recently the provided Makefile_Cygwin file was used to successfully build **falcon** with Cygwin DLL version 3.4.9 on a Windows 10 Pro version 22H2 system. In addition to GNU make and gcc toolchains, the following libraries and applications are required to build and execute **falcon**, and work with captured pixel image files.

+ OpenGL You’ll need the OpenGL library for rendering. OpenGL is available on most platforms by default, but for Windows, you may need to install the appropriate drivers.
+ GLEW (OpenGL Extension Wrangler Library): Handles the management of OpenGL extensions. It's used to access newer OpenGL functions.
+ GLFW (Graphics Library Framework) or SDL: Both are commonly used for creating windows, handling input, and managing OpenGL contexts. GLFW is a lightweight option, while SDL is more comprehensive.
+ GLM (OpenGL Mathematics): A math library built for OpenGL, used for matrix and vector operations (essential for 3D transformations like translation, scaling, rotation).
+ GLSL (OpenGL Shading Language): You'll be writing vertex and fragment shaders for rendering in 3D. You should familiarize yourself with GLSL and its syntax.
+ CMake: For building your project in a cross-platform environment. It helps in managing dependencies like GLEW, GLFW, etc., across different OS.
+ ImageMagick for convert app to convert XPM to GIF/JPEG formats and create animated GIF
+ ffmpeg to create MP4 video file from JPEG image sequence
+ ffplay or other video player such as MS Windows Media Player or VLC media player.

The **falcon** program is an OpenGL application that opens a desktop window in which animated 3D rendering is displayed. On Linux and Windows systems, a running X server is required. Most Linux distributions provide and start X server prior to displaying the desktop graphical interface. On Windows a third party X server application, such as those provided by VcXsrv or Cygwin, must be installed and activated before **falcon** can run. The following web articles provide information to get VcXsrv installed and working with a Windows 10 WSL2 Linux distribution.

+ [WSL2 GUI X-Server Using VcXsrv](https://www.shogan.co.uk/how-tos/wsl2-gui-x-server-using-vcxsrv/)

If needing to use ssh or rdp to connect to a running WSL Linux instance, having the following /etc/wsl.conf file on the installed Linux distribution is convenient.

```
[boot]
#systemd=true
command="service dbus start; service ssh start; service xrdp start"
```
Consult the online [Cygwin/X User's Guide](https://x.cygwin.com/docs/ug/cygwin-x-ug.html) for instructions on installing, configuring and using Cygwin X server on a Windows platform.

## Execution Overview ##

The following steps delineate typical **falcon** utilization from a bash shell terminal.

### 1. Build falcon executable ###
From within the ./falcon directory, invoke **./Make_falcon** to build "./bin/falcon.exe".

This step only needs to be performed when the **falcon** repository is first installed, and after any code changes have been made to **falcon** source files.

### 2. Render animation and capture image sequence ###
From within the ./falcon directory, invoke **./Exec_falcon** with a specified TXYZ.OUT case number (i.e.,  0000, 0001, 0002 or 0003).

A blank falcon display window should appear on the desktop and keypress options to control the animation printed in the terminal window. Animation starts by clicking a mouse button when the mouse cursor is placed within the falcon display window. Keypresses are only effective if the mouse cursor is within the display window. Pressing the "Esc" key or closing the falcon display window will terminate the program.

### 3. Create animated GIF or MP4 video ###
Upon termination of the **falcon** program, change to the ./Ximg subdirectory and invoke **../util/xpm2gif** to create animated GIF file named "img_anim.gif", or invoke **../util/xpm2mp4** to create MP4 video file named "img_anim.mp4".

Note, to prevent captured and converted images from a previous **falcon** execution being incorporated into a subsequent animated GIF or MP4 video file, it's recommended all img_####.\[xpm|gif|jpg] files be deleted from the ./Ximg subdirectory before beginning another **falcon** execution. Also, rename or move the newly created animated GIF or MP4 video file to prevent it being overwritten. 

## Caveats ##

All input facet model shape polygon data file paths are hard coded in the draw3D.c file. This was done to shift the task of assigning file paths to OS shell scripts external to **falcon** for batch processing management. Although the program is now interactive, batch processing use cases were not human-in-the-loop.

Quadrilateral polygons are used for shape facets, instead of triangles as in modern surface mesh models, since it was easier and faster to decompose missile and target shapes, and ground plane grids into quad patches. Also, there was no requirement to calculate accurate surface normals to model realistic lighting effects or perform face culling. Each facet is assigned a constant fill color, and considered visible on both surfaces. Consistency in direction around perimeter of a shape model facet that polygon vertice are specified should be maintained to facilitate possibilty of face culling being introduced in future rendering enhancements. Currently, the order adheres to right-hand-rule such that the positive normal is in a facet's +Z direction.

There is no attempt to resolve problems rendering intersecting polygons. Other than time of intercept, as missile impacts target, adjoined polygons do not intersect others.

The provided TXYZ.OUT.000# trajectory files generated by a Mathcad 3-DOF kinematic model of an ideal (no control lag with 100% effective, but bounded commanded acceleration and perfect command response) pure proportional navigation (N=4) guided missile correspond to the following engagement scenarios for a typical MANPADS type SAM.

+ 0000 - Fixed-wing target at constant speed, altitude and heading; SAM launched with 12 degree elevation and 10 degree lead azimuth.
+ 0001 - Fixed-wing target performing a constant 2g banking level turn toward its left; SAM launched with 12 degree elevation and 10 degree lead azimuth.
+ 0002 - Fixed-wing target performing a constant 2g banking descending turn toward its left; SAM launched with 12.6 degree elevation and 27 degree lead azimuth.
+ 0003 - Fixed-wing target performing a constant 3g banking descending turn toward its left; SAM launched with 14 degree elevation and 27.2 degree lead azimuth.

In each case the SAM, with constant velocity of 450 meters/second, was launched against the target flying at 200 meters/second, initially located at 2000 meters downrange, 500 meters height above ground level, and heading 90 degrees left of the missile launch location. For a sense of relative dimensions of missile and target, the fixed-wing target shape model is scaled to about the size of a Piper M350 aircraft.


## References ##

\[1] David F. Rogers,"Procedural Elements for Computer Graphics" McGraw-Hill, Inc., 1985.

\[2] Robert Sedgewick and Keven Wayne, "Algorithms, 4th edition", Addison Wesley Professional, March 23, 2011. [Web available at algs4.cs.princeton.edu](http://algs4.cs.princeton.edu/home/)

## Disclaimers ##

This project is a refactored version of the original threeD project, converted from C to C++ and integrated with OpenGL for rendering. The following points should be noted:

Experimental Nature: This project is an experimental adaptation of the original code and may contain modifications that differ from the initial design. The code is provided as-is, and there is no guarantee of full functionality or performance equivalence to the original implementation.

Platform-Specific Behavior: While this project is designed to work with OpenGL, compatibility with different systems and devices (such as STM32MP157 or other embedded platforms) may vary. Proper OpenGL ES support and adjustments for embedded platforms are necessary, and users are responsible for ensuring compatibility with their target hardware.

Third-Party Libraries: This project may require external libraries such as GLFW or GLEW to handle OpenGL context creation and management. Users are responsible for installing and configuring these dependencies.

Limited Support: No official support is provided for this refactored version, and it is intended for educational and research purposes. Contributions and modifications are welcome, but they are not guaranteed to be merged into the repository.  

Licensing: The project continues to follow the licensing terms of the original threeD repository. Users must comply with the licensing requirements specified in the original codebase.

## Epilogue ##

The Falcon project has been an incredible learning experience, not just for the technical challenges it posed, but for the sense of discovery it brought. What started as an ambitious idea to bring real-time 3D rendering to an embedded platform grew into something far more meaningful. It taught me how to push the boundaries of the STM32MP157 MCU, adapting OpenGL’s power to a constrained environment and finding creative solutions along the way.

Each step—integrating GLEW and GLFW, working through shader intricacies, and optimizing performance—was a test of patience and problem-solving. There were frustrating moments, but each hurdle overcome was a reminder of why I love building things like this. It wasn’t just about making something work; it was about understanding how to make it better, more efficient, and adaptable.

As I close the Falcon project, I do so with a sense of pride and fulfillment. This is more than just code; it’s a testament to what’s possible when passion meets perseverance. While this project now comes to a close, the ideas and skills I’ve gained here will undoubtedly fuel future endeavors. This journey has only just begun.
