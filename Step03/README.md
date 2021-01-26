#Install 
cmake --install . 

or overwrite install folder prefix argument to determine the installation folder

cmake --install . --prefix "/path/to/install/to"

#Run tests
##for Debug
ctest -C Debug -VV
##for Release
ctest -C Release -VV