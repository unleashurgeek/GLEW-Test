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
            "In-Source builds are not allowed. To ensure proper GIT encapsulation the cmake must be run out-of-source in one of the following locations:\n   /build, /vs, /vs10, /vs11, /vs12, /vs13, vs14, or /vs15.\nWhen in one of the out-of-source directories pass the root path as the last argument (ex: cmake ../)\nThis process created the 'CMakeCache.txt' and the directory 'CMakeFiles'. Please delete them.")
endif()
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/build"  _inbuild)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/vs" _invs)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/vs10" _invs10)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/vs11" _invs11)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/vs12" _invs12)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/vs13" _invs13)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/vs14" _invs14)
string(COMPARE EQUAL cmake_binary_lower "${cmake_source_lower}/vs15" _invs15)
if (NOT _inbuild AND _invs AND _invs10 AND _invs11 AND _invs12 AND _invs13 AND _invs14 AND _invs15)
    message(FATAL_ERROR
            "Out-of-source builds that are not in /build, /vs, /vs10, /vs11, /vs12, /vs13, vs14, or /vs15 are not allowed. This is to ensure proper GIT encapsulation. The cmake must be run out-of-source in one of the following locations:\n   /build, /vs, /vs10, /vs11, /vs12, /vs13, vs14, or /vs15.\nThis process created the 'CMakeCache.txt' and the directory 'CMakeFiles'. Please delete them.")
endif()

#--------------------------------------------------------------------
# Add subdirectories
#--------------------------------------------------------------------
add_subdirectory(deps/GLEW)
add_subdirectory(deps/GLFW)

add_subdirectory(src)