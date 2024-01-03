cmake_minimum_required(VERSION 3.15)

project(qwt)

if (MSVC)
    # Disable some annoying warnings (relative to warning level 3)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4267 /MP")
endif()

# Adapted from src.pri file in Qwt's source catalog for Qwt 6.1.0
# Compiles static library
# Some bits removed (at least SVG)

add_definitions(-DQWT_NO_SVG -DQWT_MOC_INCLUDE)

include(src/CMakeLists.txt)

option(USE_QT6 "Use Qt6" FALSE)

if(USE_QT6)
  find_package(Qt6 COMPONENTS REQUIRED Concurrent Core Gui OpenGLWidgets PrintSupport Widgets)
  set(QT_LIBRARIES Qt6::Concurrent Qt6::Core Qt6::Gui Qt6::OpenGLWidgets Qt6::PrintSupport Qt6::Widgets)
else(USE_QT6)
  find_package(Qt5 COMPONENTS REQUIRED Concurrent Core Gui OpenGL PrintSupport Widgets)
  set(QT_LIBRARIES Qt5::Concurrent Qt5::Core Qt5::Gui Qt5::OpenGL Qt5::PrintSupport Qt5::Widgets)
endif(USE_QT6)


set(CMAKE_AUTOMOC ON)
add_library(${PROJECT_NAME} src/CMakeLists.txt ${HEADER_FILES} ${SOURCE_FILES} )

target_include_directories(${PROJECT_NAME}
  PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/src
)

target_link_libraries(${PROJECT_NAME} ${QT_LIBRARIES})
