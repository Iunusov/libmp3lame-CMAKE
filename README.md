CMake files for development needs , because the official repo supports only makefiles.

Basically, can be used as CMake subproject with static linking.

#How to build 
```bash
mkdir cmake
cd cmake
cmake ..
make (or refer to cmake dir, if you are using Visual Studio)

or just add that library to your CMake project using add_subdirectory() and target_link_libraries()
