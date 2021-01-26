#Packaging Debug and Release
This example is valid for single-configuration generators and will not work for multi-configuration generators (e.g. Visual Studio).

By default, CMake’s model is that a build directory only contains a single configuration, be it Debug, Release, MinSizeRel, or RelWithDebInfo. 
It is possible, however, to setup CPack to bundle multiple build directories and construct a package that contains multiple configurations of the same project.