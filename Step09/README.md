#Static Libraries

mkdir build
cd build

NOTE: see MathFunctions/CMakeLists.txt to distinguish static and shared lib
cmake .. -DUSE_MYMATH=On

cmake --build . --config Release
cmake --install . --prefix ./install
