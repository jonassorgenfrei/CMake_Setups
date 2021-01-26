# minimum required version
cmake_minimum_required (VERSION 3.10)

# project name and version
project (Tutorial VERSION 9.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# control where the static and shared libraries are built so that on windows
# we don't need to tinker with the path to run the executable
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")

option(BUILD_SHARED_LIBS "Build using shared libraries" ON)

# configure a header file to pass some of the CMake settings to the source code
configure_file (TutorialConfig.h.in TutorialConfig.h)

#  add the MathFunctions library
# the math functions subdirectory will always be included
add_subdirectory(MathFunctions)

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries (Tutorial PUBLIC MathFunctions)

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC "${PROJECT_BINARY_DIR}")

##########
## INSTALLING
##########

# add the install targets
install (TARGETS Tutorial DESTINATION bin COMPONENT applications)
install (FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h" DESTINATION include COMPONENT headers)

##########
## TESTING
##########

# enable dashboard scripting
# automatically calls enable_testing()
include(CTest)

# does the application run
add_test(NAME Runs COMMAND Tutorial 25)

# does the usage message work?
add_test(NAME Usage COMMAND Tutorial)
set_tests_properties(Usage PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number")

# define a function to simplify adding tests
function(do_test target arg result)
  add_test(NAME Comp${arg} COMMAND ${target} ${arg})
  set_tests_properties(Comp${arg}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result}
	)
endfunction(do_test)


# do a bunch of result based tests
do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is [-nan|nan|0]")
do_test(Tutorial 0.0001 "0.0001 is 0.01")


# budilding an installer using Cpack
####################################

# this module will include any runtime libraries 
# that are needed by the project for the current platform
include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_NAME "Tutorial")
set(CPACK_PACKAGE_VENDOR "CMake.org")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Tutorial Application - CPack Component Installation Example")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "Tutorial Application")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/License.txt")

set(CPACK_PACKAGE_VERSION "1.0.0")
set(CPACK_PACKAGE_VERSION_MAJOR "${Tutorial_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${Tutorial_VERSION_MINOR}")

# Define components and their display names
set(CPACK_COMPONENTS_ALL applications libraries headers)
set(CPACK_COMPONENT_APPLICATIONS_DISPLAY_NAME "Tutorial Applications")
set(CPACK_COMPONENT_LIBRARIES_DISPLAY_NAME "Libraries")
set(CPACK_COMPONENT_HEADERS_DISPLAY_NAME "C++ Headers")

# Human readable component descriptions
set (CPACK_COMPONENT_APPLICATIONS_DESCRIPTION "An extremely useful application that makes use of Tutorial")
set (CPACK_COMPONENT_LIBRARIES_DESCRIPTION "Static libraries used to build programs with Tutorial")
set (CPACK_COMPONENT_HEADERS_DESCRIPTION "C/C++ header files for use with Tutorial")

# Define dependencies between components
set (CPACK_COMPONENT_HEADERS_DEPENDS libraries)

# Define groups
set(CPACK_COMPONENT_APPLICATIONS_GROUP "Runtime")
set(CPACK_COMPONENT_LIBRARIES_GROUP "Development")
set(CPACK_COMPONENT_HEADERS_GROUP "Development")

set(CPACK_COMPONENT_GROUP_DEVELOPMENT_DESCRIPTION "All of the tools you'll ever need to develop software")

# Define NSIS installation types
set(CPACK_ALL_INSTALL_TYPES Full Developer)
set(CPACK_COMPONENT_LIBRARIES_INSTALL_TYPES Developer Full)
set(CPACK_COMPONENT_HEADERS_INSTALL_TYPES Developer Full)
set(CPACK_COMPONENT_APPLICATIONS_INSTALL_TYPES Full)

include(CPack)