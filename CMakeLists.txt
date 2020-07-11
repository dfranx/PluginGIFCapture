cmake_minimum_required(VERSION 3.1)
project(GIFCapture)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ./bin)

# source code
set(SOURCES
	dllmain.cpp
	GIFCapture.cpp

# libraries
	libs/imgui/imgui_draw.cpp
	libs/imgui/imgui_widgets.cpp
	libs/imgui/imgui.cpp
)

# cmake toolchain 
if(CMAKE_TOOLCHAIN_FILE)
    include(${CMAKE_TOOLCHAIN_FILE})
endif(CMAKE_TOOLCHAIN_FILE)

# opengl
find_package(OpenGL REQUIRED)

# glew
find_package(GLEW REQUIRED)

# create executable
add_library(GIFCapture SHARED ${SOURCES})

set_target_properties(GIFCapture PROPERTIES OUTPUT_NAME "plugin")
set_target_properties(GIFCapture PROPERTIES PREFIX "")

target_include_directories(GIFCapture PRIVATE ${GLEW_INCLUDE_DIRS} ${OPENGL_INCLUDE_DIRS} libs inc)

# link libraries
target_link_libraries(GIFCapture ${OPENGL_LIBRARIES})

if(WIN32)
	target_link_libraries(GIFCapture GLEW::GLEW)
else()
	target_link_libraries(GIFCapture ${GLEW_LIBRARIES})
endif()

if (NOT MSVC)
	target_compile_options(GIFCapture PRIVATE -Wno-narrowing)
endif()