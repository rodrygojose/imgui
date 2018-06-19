CMAKE_MINIMUM_REQUIRED(VERSION 3.5)

################################################################################
# Compiles imgui library using GLEW + GLFW3
# It defines:
# IMGUI_INCLUDE = include directory of imgui
# IMGUI_LIB = linking directory of imgui
################################################################################

PROJECT(imgui_impl_opengl3)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -W -std=c++11")

IF(MSVC)
  SET(BUILD_SHARED_LIBS OFF)
ELSE()
  OPTION(BUILD_SHARED_LIBS "Build shared libraries" OFF)
  OPTION(BUILD_POSITION_INDEPENDENT_CODE "Build position independent code (-fPIC)" ON)
ENDIF()

IF(BUILD_POSITION_INDEPENDENT_CODE)
  ADD_DEFINITIONS( -fPIC )
ENDIF()

#-------------------------------------------------------------------------------

FIND_PACKAGE(OpenGL REQUIRED)

FIND_PACKAGE(glfw3 REQUIRED)

IF (NOT APPLE)
    FIND_PACKAGE(GLEW)
ENDIF()

#-------------------------------------------------------------------------------

FILE(GLOB IMGUI_HEADERS ./*.h)

FILE(GLOB IMGUI_SOURCE ./*.cpp)

SET(IMGUI_IMPL
    ./examples/imgui_impl_glfw.cpp
    ./examples/imgui_impl_opengl3.cpp
    ./examples/imgui_impl_glfw.h
    ./examples/imgui_impl_opengl3.h)

SET(LIBS
    ${OPENGL_LIBRARIES}
    ${GLEW_LIBRARY}
    glfw)

#-------------------------------------------------------------------------------

ADD_LIBRARY(imgui
  ${IMGUI_HEADERS}
  ${IMGUI_SOURCE}
  ${IMGUI_IMPL})

INCLUDE_DIRECTORIES(
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${GLEW_INCLUDE_DIR}
  ${OPENGL_INCLUDE_DIR})

TARGET_LINK_LIBRARIES(imgui ${LIBS})

#-------------------------------------------------------------------------------

INSTALL(TARGETS imgui
    ARCHIVE DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/lib
    LIBRARY DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/lib
    RUNTIME DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/bin
    COMPONENT library)

INSTALL(FILES ${IMGUI_HEADERS}
    DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/include)

#-------------------------------------------------------------------------------

SET(IMGUI_INCLUDE
   ${CMAKE_CURRENT_SOURCE_DIR}
   ${CMAKE_CURRENT_SOURCE_DIR}/examples
   ${CMAKE_CURRENT_BINARY_DIR}/include
   CACHE INTERNAL "IMGUI_INCLUDE")

SET(IMGUI_LIB
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}/lib
    CACHE INTERNAL "IMGUI_LIB")

#--------------------------------------------------------------------------------
