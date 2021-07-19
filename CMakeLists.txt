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

# set specific source for platform
if (WIN32)
    file (GLOB PLATFORM_SRC
        ./imgui-1.83/backends/imgui_impl_win32.*
        ./imgui-1.83/backends/imgui_impl_dx12.*
        ./src/win/main.cpp
    )
elseif (APPLE)
    file (GLOB PLATFORM_SRC
        ./imgui-1.83/backends/imgui_impl_metal.*
        ./imgui-1.83/backends/imgui_impl_osx.*
        ./src/mac/main.cpp
    )
elseif (UNIX AND NOT APPLE)
    file (GLOB PLATFORM_SRC
        ./imgui-1.83/examples/libs/gl3w/GL/gl3w.*
        ./imgui-1.83/backends/imgui_impl_glfw.*
        ./imgui-1.83/backends/imgui_impl_opengl3.*
        ./src/linux/main.cpp
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
elseif (APPLE)
elseif (UNIX AND NOT APPLE)
# should install glfw first: sudo apt install libglfw3-dev
target_link_libraries(${PROJECT_NAME}
    glfw3
)
endif()
