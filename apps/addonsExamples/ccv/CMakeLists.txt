cmake_minimum_required(VERSION 2.8)
# gess project name from current directory name
get_filename_component(PROJECT_ROOT ${CMAKE_CURRENT_LIST_FILE} PATH)                                                                                            
get_filename_component(PROJECT_NAME ${PROJECT_ROOT} NAME)

project(${PROJECT_NAME})

include(FindPkgConfig)                                                                                                                                        
pkg_check_modules(OF REQUIRED openFrameworks)

# depends on these addons:
include(../../../../openFrameworks/addons/ofxDirList/ofxDirList.cmake)
include(../../../../openFrameworks/addons/ofxNCore/ofxNCore.cmake)
include(../../../../openFrameworks/addons/ofxNetwork/ofxNetwork.cmake)
include(../../../../openFrameworks/addons/ofxOpenCv/ofxOpenCv.cmake)
include(../../../../openFrameworks/addons/ofxOsc/ofxOsc.cmake)
include(../../../../openFrameworks/addons/ofxThread/ofxThread.cmake)
include(../../../../openFrameworks/addons/ofxVectorGraphics/ofxVectorGraphics.cmake)
include(../../../../openFrameworks/addons/ofxVectorMath/ofxVectorMath.cmake)
include(../../../../openFrameworks/addons/ofxXmlSettings/ofxXmlSettings.cmake)

file( GLOB_RECURSE PROJECT_SOURCES src/*.cpp)

include_directories(${OF_INCLUDE_DIRS} ${ADDONS_INCLUDE_DIRS} src)
add_executable(${PROJECT_NAME} ${PROJECT_SOURCES} ${ADDONS_SOURCES})
target_link_libraries(${PROJECT_NAME} ${OF_LIBRARIES} ${ADDONS_LIBRARIES})
