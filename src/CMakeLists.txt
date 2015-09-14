
link_libraries(glfw glew)
if (BUILD_SHARED_LIBS)
    add_definitions(-DGLFW_DLL -DGLEW_DLL)
endif()

include_directories("${GLFW_SOURCE_DIR}/include"
                    "${GLEW_SOURCE_DIR}/include")

add_executable(Test main.c)
