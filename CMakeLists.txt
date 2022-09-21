cmake_minimum_required(VERSION 3.5)
project(gflags_vendor)

find_package(ament_cmake REQUIRED)

include(ExternalProject)

file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include)

ExternalProject_Add(
  gflags_src
  URL https://github.91chi.fun//https://github.com/gflags/gflags/archive/v2.2.1.zip
  URL_MD5 2d988ef0b50939fb50ada965dafce96b
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cd ../gflags_src &&
     cmake .
       -DCMAKE_INSTALL_PREFIX:PATH=${CMAKE_CURRENT_BINARY_DIR}/gflags_install
       -DBUILD_SHARED_LIBS:BOOL=OFF
       -DGFLAGS_NAMESPACE:STRING=google
       -DCMAKE_BUILD_TYPE:STRING=Release
       -DCMAKE_TOOLCHAIN_FILE:STRING=${CMAKE_TOOLCHAIN_FILE}
  BUILD_COMMAND cd ../gflags_src && cmake --build . --config Release
  INSTALL_COMMAND cd ../gflags_src && cmake --install .
)

install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/gflags_install/include/
        DESTINATION ${CMAKE_INSTALL_PREFIX}/include
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/gflags_install/lib/
        DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
        FILES_MATCHING PATTERN "*gflags*")
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/gflags_src-prefix/src/gflags_src/src/gflags_completions.sh
        PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
        DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)

ament_package(CONFIG_EXTRAS "gflags_vendor-extras.cmake.in")