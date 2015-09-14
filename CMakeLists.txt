project (GLTest CXX)

cmake_minimum_required(VERSION 3.3.1)

set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "Build the GLFW example programs")
set(GLFW_BUILD_TESTS OFF CACHE BOOL "Build the GLFW test programs")
set(GLFW_BUILD_DOCS OFF CACHE BOOL "Build the GLFW documentation")

set(GLEW_BUILD_INFO OFF CACHE BOOL "Build glewinfo & visualinfo binaries")

#--------------------------------------------------------------------
# Verify cmake is being run out-of-source
# and in the /build or /vs directory.
#--------------------------------------------------------------------
string(TOLOWER "${CMAKE_BINARY_DIR}" cmake_binary_lower)
string(TOLOWER "${CMAKE_SOURCE_DIR}" cmake_source_lower)
string(COMPARE EQUAL "${CMAKE_SOURCE_DIR}" "${CMAKE_BINARY_DIR}" _insource)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/build"  _inbuild)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/vs" _invs)
if (_insource)
    message(FATAL_ERROR
            "In-Source builds are not allowed. To ensure proper GIT encapsulation the cmake must be run out-of-souce in either /build or /VS. When in that directory pass the root path as the last argument (ex: cmake ../). \nThis process created the 'CMakeCache.txt' and the directory 'CMakeFiles'. Please delete them.")
endif()
if (NOT _inbuild AND _invs)
    message(FATAL_ERROR
            "Out-of-source builds that are not in /build or /vs are not allowed. This is to ensure proper GIT encapsulation. The cmake must be run out-of-souce in either /build or /VS. \nThis process created the 'CMakeCache.txt' and the directory 'CMakeFiles'. Please delete them.")
endif()

#--------------------------------------------------------------------
# Add subdirectories
#--------------------------------------------------------------------
add_subdirectory(deps/GLEW)
add_subdirectory(deps/GLFW)

add_subdirectory(src)