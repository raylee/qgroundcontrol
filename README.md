# QGroundControl 
## Open Source Micro Air Vehicle Ground Control Station


* Project:
<http://qgroundcontrol.org>

* Files:
<http://github.com/mavlink/qgroundcontrol>

* Credits:
<http://qgroundcontrol.org/credits>


## Documentation
For generating documentation, refer to /doc/README.

## Notes
Please make sure to delete your build folder before re-building. Independent of which 
build system you use (this is not related to Qt or your OS) the dependency checking and 
cleaning is based on the current project revision. So if you change the project and don't remove the build folder before your next build, incremental building can leave you with stale object files.

## Additional functionality
QGroundcontrol has functionality that is dependent on the operating system and libraries installed by the user. The following sections describe these features, their dependencies, and how to disable/alter them during the build process.

### QUpgrade
QUpgrade is a submodule (a Git feature like a sub-repository) that contains extra functionality. It is compiled in by default if it has initialized and updated. It can be disabled by specifying the DISABLE_QUPGRADE definition when calling qmake `qmake DEFINES=DISABLE_QUPGRADE`. Note that multiple defines can be specified like this: `qmake DEFINES="DISABLE_QUPGRADE DISABLE_SPEECH"`.

To include QUpgrade functionality run the following (only needs to be done once after cloning the qggroundcontrol git repository):
  * `git submodule init`
  * `git submodule update`

The QUpgrade module relies on `libudev` on Linux platforms.

### Specifying MAVLink dialects
The MAVLink dialect compiled by default by QGC is for the ardupilotmega. This will happen if no other dialects are specified. To override this create a `user_config.pri` file in the root directory and set the `MAVLINK_CONF` variable using qmake's variable notation syntax: `MAVLINK_CONF=sensesoar`. This variable can be a list of dialects that should all be supported like `MAVLINK_CONF=sensesoar ardupilotmega`. Note that doing this may result in compilation errors as certain dialects may conflict with each other!

The `MAVLINK_CONF` variable can also be specified at the command line as an argument to qmake to allow for easy one-off compilations: `qmake MAVLINK_CONF="sensesoar ardupilotmega"`

### Speech syntehsis
QGroundcontrol can notify the controller of information via speech synthesis on the Mac and Linux platforms. This requires the `flite` library on Linux while on Mac text-to-speech support is built in starting with OS 10.6+ (Snow Leopard). This support is enabled by default on all platforms if the dependencies are met. Disabling this functionality can be done by adding the `DISABLE_SPEECH` define when running `qmake` like: `qmake DEFINES=DISABLE_SPEECH`. Note that multiple defines can be specified like this: `qmake DEFINES="DISABLE_QUPGRADE DISABLE_SPEECH"`.

### XBee support
QGroundControl can talk to XBee wireless devices using their proprietary protocol directly on Windows and Linux platforms. This support is not necessary if you're not using XBee devices or aren't using their proprietary protocol. On Windows, the necessary dependencies are included in this repository and no additional steps are required. For Linux, change to the `libs/thirdParty/libxbee` folder and run `make;sudo make install` to install libxbee on your system (uninstalling can be done with a `sudo make uninstall`). qmake will automatically detect the library on Linux, so no other work is necessary.

To disable XBee support you may specify `DISABLE_XBEE` in the DEFINES argument to qmake like `qmake DEFINES=DISABLE_XBEE`. Multiple options may be specified for `DEFINES` like: `qmake DEFINES="DISABLE_XBEE DISABLE_SPEECH".

# Build on Mac OSX

To build on Mac OSX (10.6 or later):
- - -
### Install SDL

1. Download SDL from:  <http://www.libsdl.org/release/SDL-1.2.14.dmg>
2. From the SDL disk image, copy the `sdl.framework` bundle to `/Library/Frameworks` directory (if you are not an admin copy to `~/Library/Frameworks`)

### Install QT
- - -
1. Download Qt 4.8+ from <http://download.qt-project.org/official_releases/qt/4.8/4.8.5/qt-mac-opensource-4.8.5.dmg > 
2. Double click the package installer and follow instructions: <http://qt-project.org/doc/qt-4.8/install-mac.html>

### Build QGroundControl
- - -
 (use clang compiler - not gcc)

1. From the terminal go to the `groundcontrol` directory
2. Run `qmake qgroundcontrol.pro -r -spec unsupported/macx-clang CONFIG+=x86_64`
3. Run `make -j4`


# Build on Linux 


To build on Linux:
- - -
1. Install base dependencies (QT + phonon/webkit, SDL)
  * For Ubuntu: `sudo apt-get install libqt4-dev libphonon-dev libphonon4 phonon-backend-gstreamer qtcreator libsdl1.2-dev build-essential`
  * For Fedora: `sudo yum install qt qt-creator qt-webkit-devel SDL-devel SDL-static systemd-devel`

2. **[OPTIONAL]** Install additional libraries
  * For text-to-speech (flite)
	* For Ubuntu: `sudo apt-get install libflite1 flite1-dev`
	* For Fedora: `sudo yum install flite-devel`
  * For 3D flight view (openscenegraph)
	* For Ubuntu: `sudo apt-get install libopenscenegraph-dev`
	* For Fedora: `sudo yum install OpenSceneGraph-qt-devel`

3. Clone the repository
  1. `cd PROJECTS_DIRECTORY`
  2. git clone https://github.com/mavlink/qgroundcontrol.git
  3. **[OPTIONAL]** For QUpgrade integration:
	1. `cd qgroundcontrol`
	2. `git submodule init`
	3. `git submodule update`

4. **[OPTIONAL]** Build and install XBee support:
  1. ` cd libs/thirdParty/libxbee`
  2. `make`
  3. `sudo make install`

5. Build QGroundControl:
  1. Go back to root qgroundcontrol directory
  2. `qmake`
  3. `make`
	* To enable parallel compilation add the `-j` argument with the number of cores you have. So on a quad-core processor: `make -j4`

6. Run qgroundcontrol
  1. `./release/qgroundcontrol`

# Build on Windows
- - -

__GNU GCC / MINGW IS UNTESTED, COULD WORK WITH
VISUAL STUDIO 2008 / 2010 EXPRESS EDITION (FREE!)__

Steps for Visual Studio 2008 / 2010:

Windows XP/7:

1. Download and install the Qt libraries for Windows from https://qt.nokia.com/downloads/ (the Visual Studio 2008 or 2010 version as appropriate)

2. Download and install Visual Studio 2008 or 2010 Express Edition (free) from https://www.microsoft.com/visualstudio. If using Visual Studio 2010, make sure you are running at least SP1. There is a linking error you'll encounter otherwise that will prevent compilation.

3. Go to the QGroundControl folder and then to thirdParty/libxbee and build it following the instructions in win32.README

4. Open the Qt Command Prompt program (should be in the Start Menu), navigate to the source folder of QGroundControl and create the Visual Studio project by typing `qmake -tp vc qgroundcontrol.pro`

5. Now start Visual Studio and load the qgroundcontrol.vcproj if using Visual Studio 2008 or qgroundcontrol.vcxproj if using Visual Studio 2010

6. Compile and edit in Visual Studio. If you need to add new files, add them to qgroundcontrol.pro and re-run `qmake -tp vc qgroundcontrol.pro`


## Repository Layout
    qgroundcontrol:
	    demo-log.txt
	    license.txt 
	    qgcunittest.pro - For the unit tests.
	    qgcunittest.pro.user
	    qgcvideo.pro
	    qgroundcontrol.pri - Used by qgroundcontrol.pro
	    qgroundcontrol.pro - Project opened in QT to run qgc.
	    qgroundcontrol.pro.user 
	    qgroundcontrol.qrc - Holds many images.
	    qgroundcontrol.rc - line of code to point toward the images
	    qserialport.pri - generated by qmake.
	    testlog.txt - sample log file
	    testlog2.txt - sample log file
	    user_config.pri.dist - Custom message specs to be added here. 
    data: 
	    Maps from yahoo and kinect and earth. 
    deploy: 
	    Install and uninstall for win32.
	    Create a debian packet.
	    Create .DMG file for publishing for mac.
	    Audio test on mac.	
    doc: 
	    Doxyfile is in this directory and information for creating html documentation for qgc.
    files: 
	    Has the audio for the vehicle and data output. 
		    ardupilotmega: 
			    widgets and tool tips for pilot heading for the fixed wing.
			    tooltips for quadrotor
		    flightgear:
			    Aircraft: 
				    Different types of planes and one jeep. 
			    Protocol: 
				    The protocol for the fixed_wings and quadrotor and quadhil.holds info about the fixed wing yaw, roll etc. 	
			    Quadrotor:
			        Again holds info about yaw, roll etc.
			    Pixhawk:
			        Widgets for hexarotor. Widgets and tooltips for quadrotor.
			    vehicles: 
			        different vehicles. Seems to hold the different kinds of aircrafts as well as files for audio and the hexarotor and quadrotor.
			    widgets: 
			        Has a lot of widgets defined for buttons and sliders.

    images: 
	    For the UI. Has a bunch of different images such as images for applications or actions or buttons.
    lib: 
	    SDL is located in this direcotry. 
	Msinttypes: 
	    Defines intteger types for microsoft visual studio. 
	sdl:
	    Information about the library and to run the library on different platforms. 
    mavlink: 
	    The files for the library mavlink. 
    qgcunittest: 
	    Has the unittests for qgc
    settings: 
	    Parameter lists for alpha, bravo and charlie. Data for stereo, waypoints and radio calibration. 
    src:
	    Code for QGCCore, audio output, configuration, waypoints, main and log compressor.
	apps
	    Code for mavlink generation and for a video application.
	comm
	    Code for linking to simulation, mavlink, udp, xbee, opal, flight gear and interface.
	Has other libraries. Qwt is in directory named lib. The other libraries are in libs.
	lib
	    qwt library
	libs
	    eigen, opmapcontrol, qestserialport, qtconcurrent, utils.
	input
	    joystick and freenect code.
	plugins
	    Qt project for PIXHAWK plugins.
	uas
	    Ardu pilot, UAS, mavlink factory, uas manager, interface, waypoint manager and slugs.
	ui
	    Has code for data plots, waypoint lists and window congfiguration. All of the ui code.
thirdParty: 
	    Library called lxbee.
	    Library called QSerialPort.
