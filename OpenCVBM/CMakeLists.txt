cmake_minimum_required(VERSION 3.5)

PROJECT( OpenCVBM )
FIND_PACKAGE( OpenCV REQUIRED )
FIND_PACKAGE( Qt5 REQUIRED COMPONENTS Core Widgets )

#set(CMAKE_BUILD_TYPE Debug)

SET( OpenCVBM_SOURCES src/main.cpp
                        src/OpenCVBMWindow.cpp
                        src/blockmatchingimage.cpp
                        src/blockmatching.cpp
                        src/sgblockmatching.cpp)
SET( OpenCVBM_HEADERS src/OpenCVBMWindow.h
                        src/blockmatchingimage.h
                        src/blockmatching.h
                        src/sgblockmatching.h)
SET( OpenCVBM_FORMS src/OpenCVBMWindow.ui
                        src/blockmatchingimage.ui)

qt5_wrap_cpp( OpenCVBM_HEADERS_MOC ${OpenCVBM_HEADERS} )
qt5_wrap_ui( OpenCVBM_FORMS_HEADERS ${OpenCVBM_FORMS} )

add_executable( OpenCVBM ${OpenCVBM_SOURCES} 
    ${OpenCVBM_HEADERS_MOC} 
    ${OpenCVBM_FORMS_HEADERS} )

INCLUDE_DIRECTORIES( ${CMAKE_CURRENT_BINARY_DIR} )

TARGET_LINK_LIBRARIES( OpenCVBM Qt5::Core Qt5::Widgets ${OpenCV_LIBS} )
