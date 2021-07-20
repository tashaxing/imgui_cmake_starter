cmake_minimum_required(VERSION 3.0)

project(imgui_cmake_starter)

if (UNIX)
	add_definitions(-std=c++11)
endif()

# add header path
include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/imgui-1.83
	${CMAKE_CURRENT_SOURCE_DIR}/imgui-1.83/backends
)

# set common source
file(GLOB SRC
    ./imgui-1.83/*.h
    ./imgui-1.83/*.cpp
)

# set specific source and other option for platform
if (WIN32)
    file (GLOB PLATFORM_SRC
        ./imgui-1.83/backends/imgui_impl_win32.*
        ./imgui-1.83/backends/imgui_impl_dx12.*
        ./src/win/main.cpp
    )
elseif (UNIX)
    # support both mac and linux
    add_definitions(-DIMGUI_IMPL_OPENGL_LOADER_GL3W)

    include_directories(
        ${CMAKE_CURRENT_SOURCE_DIR}/imgui-1.83/examples/libs/gl3w # for GL/gl3w.h
    )

    file (GLOB PLATFORM_SRC
        ./imgui-1.83/examples/libs/gl3w/GL/gl3w.*
        ./imgui-1.83/backends/imgui_impl_glfw.*
        ./imgui-1.83/backends/imgui_impl_opengl3.*
        ./src/unix/main.cpp
    )
endif()

# generate binary
add_executable(${PROJECT_NAME} ${SRC} ${PLATFORM_SRC})

# link lib
if (WIN32)
target_link_libraries(${PROJECT_NAME}	
    d3d12.lib
    d3dcompiler.lib
    dxgi.lib
)
elseif (UNIX)
# support both mac and linux,should install glfw first
# mac: brew install glfw3
# linux: sudo apt install libglfw3-dev
target_link_libraries(${PROJECT_NAME}
    GL
    glfw # use this lib name
    dl
)
endif()
