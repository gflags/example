cmake_minimum_required(VERSION 3.10 FATAL_ERROR)

# ------------------------------------------------------------------------------
# find / install gflags library
#
# Only one of the following alternatives is needed in your CMake project.
option(HUNTER_ENABLED "Use Hunter to download and install gflags" OFF)

## Alternative 1: Find externally installed package (e.g., apt-get, Homebrew, custom)
#
# - When gflags/CMakeLists.txt file exists, i.e., the "gflags" Git submodule was
#   initialized, run "git submodule deinit gflags" to remove the submodule files.
if (NOT HUNTER_ENABLED AND NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/gflags/CMakeLists.txt")

  project(gflags-example CXX)
  find_package(gflags REQUIRED)

## Alternative 2: Build gflags as part of this project when included as subproject
#
# - Download gflags source files using "git submodule update --init gflags".
# - In your project, gflags source files can also be committed to your own
#   project's repository using "git subtree add" instead of "git submodule".
elseif (NOT HUNTER_ENABLED AND EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/gflags/CMakeLists.txt")

  project(gflags-example CXX)
  add_subdirectory(gflags)

## Alternative 3: Using Hunter to download and install gflags when configuring project with CMake
#
# - Add cmake/HunterGate.cmake file to your project.
# - HunterGate function has to be invoked before project command.
# - hunter_add_package  has to be invoked after  project command.
else ()

  include("cmake/HunterGate.cmake")
  HunterGate(
    URL "https://github.com/schuhschuh/hunter/archive/add-gflags-version-2.2.0.tar.gz"
    SHA1 c5aeadc54dff2eb282a4da99ba03bbee8cc1f437
  )

  project(gflags-example CXX)
  hunter_add_package(gflags)
  find_package(gflags REQUIRED)

endif ()

# ------------------------------------------------------------------------------
# build example binaries
add_subdirectory(foo)
