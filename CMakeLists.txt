project (GLTest CXX)

cmake_minimum_required(VERSION 3.3.1)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "Build the GLFW example programs")
set(GLFW_BUILD_TESTS OFF CACHE BOOL "Build the GLFW test programs")
set(GLFW_BUILD_DOCS OFF CACHE BOOL "Build the GLFW documentation")

set(GLEW_BUILD_INFO OFF CACHE BOOL "Build glewinfo & visualinfo binaries")

if (MSVC)
    set(USE_MSVC_RUNTIME_LIBRARY_DLL OFF CACHE BOOL "Use MSVC runtime library DLL")
endif()

#--------------------------------------------------------------------
# Set compiler specific flags
#--------------------------------------------------------------------
if (MSVC)
    if (NOT USE_MSVC_RUNTIME_LIBRARY_DLL)
        foreach (flag CMAKE_CXX_FLAGS
                        CMAKE_CXX_FLAGS_DEBUG
                        CMAKE_CXX_FLAGS_RELEASE
                        CMAKE_CXX_FLAGS_MINSIZEREL
                        CMAKE_CXX_FLAGS_RELWITHDEBINFO)

            if (${flag} MATCHES "/MD")
                string(REGEX REPLACE "/MD" "/MT" ${flag} "${${flag}}")
            endif()
            if (${flag} MATCHES "/MDd")
                string(REGEX REPLACE "/MDd" "/MTd" ${flag} "${${flag}}")
            endif()
        endforeach()
    endif()
endif()

#--------------------------------------------------------------------
# Verify cmake is being run out-of-source
# and in the /build or /vs directory.
#--------------------------------------------------------------------
string(TOLOWER "${CMAKE_BINARY_DIR}" cmake_binary_lower)
string(TOLOWER "${CMAKE_SOURCE_DIR}" cmake_source_lower)
string(COMPARE EQUAL "${CMAKE_SOURCE_DIR}" "${CMAKE_BINARY_DIR}" _insource)
if (_insource)
    message(FATAL_ERROR
            "In-Source builds are not allowed. To ensure proper GIT encapsulation the cmake must be run out-of-source. This can be run in any created directory (ie: /build, /vs, /ide, etc).\nWhen in your out-of-source directory pass the root path as the last argument (ex: cmake ../).\nThis process created the 'CMakeCache.txt' and the directory 'CMakeFiles'. Please delete them.")
endif()
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/deps"  _indeps)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/src" _insrc)
if (_indeps OR _insrc)
    message(FATAL_ERROR
            "Builds in /src or /deps are not allowed. To ensure proper GIT encapsulation the cmake must be run out-of-source. This can be run in any created directory (ie: /build, /vs, /ide, etc) EXCEPT /src and /deps.\nWhen in your out-of-source directory pass the root path as the last argument (ex: cmake ../).\nThis process created the 'CMakeCache.txt' and the directory 'CMakeFiles'. Please delete them.")
endif()

#--------------------------------------------------------------------
# Add subdirectories
#--------------------------------------------------------------------
add_subdirectory(deps/GLEW)
add_subdirectory(deps/GLFW)

add_subdirectory(src)