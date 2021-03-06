cmake_minimum_required(VERSION 2.6)

Project(StructurePropagation)

FIND_PACKAGE(Qt4 REQUIRED)
INCLUDE(${QT_USE_FILE})

QT4_WRAP_UI(UISrcs MainWindow.ui InnerWidget.ui)
QT4_WRAP_CPP(MOCSrcs MainWindow.h InnerWidgetObject.h ComputationThread.h)

include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR})

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DQT_NO_KEYWORDS")

FIND_PACKAGE(VTK REQUIRED)
INCLUDE( ${USE_VTK_FILE} )

FIND_PACKAGE(ITK REQUIRED)
INCLUDE( ${USE_ITK_FILE} )

# OpenGM
SET(OpenGM_IncludePath "/media/portable/src/opengm10/include" CACHE STRING "Path to OpenGM include/ directory")
include_directories(${OpenGM_IncludePath})
FILE(GLOB OpenGM_Headers ${OpenGM_IncludePath}/opengm/* ${OpenGM_IncludePath}/opengm/inference/*)

ADD_EXECUTABLE(StructurePropagation main.cpp MainWindow.cpp InnerWidgetObject.cpp
vtkScribbleInteractorStyle.cxx Helpers.cpp
${MOCSrcs} ${UISrcs})
TARGET_LINK_LIBRARIES(StructurePropagation ${VTK_LIBRARIES}
${QT_LIBRARIES}
${ITK_LIBRARIES}
)

# ADD_EXECUTABLE(NonInteractive NonInteractive.cpp
# Helpers.cpp StructurePropagationBase.cpp)
# TARGET_LINK_LIBRARIES(StructurePropagation
# ITKCommon ITKIO)

#SUBDIRS( MySubDirectory )
IF(EXISTS MySubDirectory)
ADD_SUBDIRECTORY( MySubDirectory )
ENDIF(EXISTS MySubDirectory)