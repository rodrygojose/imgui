CMAKE_MINIMUM_REQUIRED(VERSION 3.5)

PROJECT(example_glfw_opengl3)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

#-------------------------------------------------------------------------------

ADD_SUBDIRECTORY(../../ ${CMAKE_CURRENT_BINARY_DIR}/imgui)

#-------------------------------------------------------------------------------

INCLUDE_DIRECTORIES(${IMGUI_INCLUDE})

LINK_DIRECTORIES(${IMGUI_LIB})

#--------------------------------------------------------------------------------

ADD_EXECUTABLE(Example_imgui_app main.cpp)

TARGET_LINK_LIBRARIES(Example_imgui_app imgui)

#--------------------------------------------------------------------------------
