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

# set source
file(GLOB SRC
	./imgui-1.83/*.h
	./imgui-1.83/*.cpp

	# backends for different platform
	if (WIN32)
		./imgui-1.83/backends/imgui_impl_win32.*
		./imgui-1.83/backends/imgui_impl_dx12.*
	elseif (APPLE)
	elseif (UNIX AND NOT APPLE)
	endif()

	# main entry
	if (WIN32)
		src/win/main.cpp
#	elseif (APPLE)
#	elseif (UNIX)
#		src/linux/main.cpp
	endif()
)

# generate binary
add_executable(${PROJECT_NAME} ${SRC})

# link lib
if (WIN32)
target_link_libraries(${PROJECT_NAME}	
	d3d12.lib
	d3dcompiler.lib
	dxgi.lib
)
elseif (UNIX)
target_link_libraries(${PROJECT_NAME}	
)
endif()