cmake_minimum_required(VERSION 3.6.2)

project(Qwt)

if (MSVC)
    # Disable some annoying warnings (relative to warning level 3)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4267")
endif()

# Adapted from src.pri file in Qwt's source catalog for Qwt 6.1.0
# Compiles static library
# Some bits removed (at least SVG)

add_definitions(-DQWT_NO_SVG)

include(src/CMakeLists.txt)

find_package(Qt5 COMPONENTS REQUIRED Concurrent Core Gui OpenGL PrintSupport Widgets)
set(QT_LIBRARIES Qt5::Concurrent Qt5::Core Qt5::Gui Qt5::OpenGL Qt5::PrintSupport Qt5::Widgets)
qt5_wrap_cpp(MOC_SOURCE_FILES ${MOC_HEADER_FILES})

add_library(${PROJECT_NAME} ${HEADER_FILES} ${SOURCE_FILES} ${MOC_SOURCE_FILES})

target_include_directories(${PROJECT_NAME}
  PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    src
)

target_link_libraries(${PROJECT_NAME} ${QT_LIBRARIES})
