#Mixing Shared Libraries

mkdir build
cd build

NOTE: see MathFunctions/CMakeLists.txt to distinguish static and shared lib
cmake .. -DCMAKE_WINDOWS_EXPORT_ALL_SYMBOLS=TRUE -DBUILD_SHARED_LIBS=On -DUSE_MYMATH=On

cmake --build . --config Release
cmake --install . --prefix ./install
