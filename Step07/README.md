#Create an Installer

Create an Installer for the developed application.

run:
mkdir build
cd build

cmake ..
cmake --build . --target package

# Notes: Windows
On Windows for CPack an install system is needed e.g. WIX or NSIS.
NSIS can be downloaded from http://nsis.sourceforge.net.

# Notes: Linux
TODO: