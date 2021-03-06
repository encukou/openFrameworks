# vim:set ft=cmake:syn on
cmake_minimum_required(VERSION 2.6)
project(openFrameworks)
set(OPENFRAMEWORKS_VERSION "0.6")

#### FIND DEPENDENCIES 
include(./../../cmake/OpenFrameworksConfig.cmake)
include(./../../cmake/OpenFrameworks.cmake)


##########################################################################

set (OFX_SOURCE_FILES 
            utils/ofUtils.cpp
            utils/ofMath.cpp
            communication/ofArduino.cpp
            communication/ofSerial.cpp
            events/ofEvents.cpp
            graphics/ofBitmapFont.cpp
            graphics/ofGraphics.cpp
            graphics/ofImage.cpp
            graphics/ofTexture.cpp
            graphics/ofTrueTypeFont.cpp
            sound/ofSoundStream.cpp
            sound/ofSoundPlayer.cpp
            app/ofAppRunner.cpp
            app/ofAppGlutWindow.cpp
            video/ofVideoPlayer.cpp
            video/ofGstUtils.cpp
            video/ofVideoGrabber.cpp
            video/ofQtUtils.cpp
            utils/ofNoise.h
            utils/ofMath.h
            utils/ofUtils.h
            utils/ofTypes.h
            utils/ofConstants.h
            events/ofEvents.h
            events/ofEventUtils.h
            communication/ofSerial.h
            communication/ofArduino.h
            ofMain.h
            graphics/ofImage.h
            graphics/ofTexture.h
            graphics/ofBitmapFont.h
            graphics/ofGraphics.h
            graphics/ofTrueTypeFont.h
            sound/ofSoundStream.h
            sound/ofSoundPlayer.h
            app/ofAppBaseWindow.h
            app/ofAppRunner.h
            app/ofAppGlutWindow.h
            app/ofBaseApp.h
            video/ofVideoPlayer.h
            video/ofQtUtils.h
            video/ofVideoGrabber.h
)
if ( "${CMAKE_SYSTEM_NAME}" MATCHES "Linux")
  if(OPENFRAMEWORKS_VIDCAP_METHOD MATCHES "gstreamer")
    set (OFX_SOURCE_FILES ${OFX_SOURCE_FILES}
      video/ofGstUtils.cpp
      video/ofGstUtils.h )
# Unicap related files where removed 
#  elseif (OPENFRAMEWORKS_VIDCAP_METHOD MATCHES "unicap")
#    set (OFX_SOURCE_FILES ${OFX_SOURCE_FILES}
#        video/ofUCUtils.cpp
#        video/ofUCUtils.h )
  endif()

endif ( )


include_directories( . 
                     app
                     communication
                     events
                     graphics
                     sound
                     utils
                     video)

set(ofincludes_files 
            utils/ofNoise.h
            utils/ofMath.h
            utils/ofUtils.h
            utils/ofTypes.h
            utils/ofConstants.h
            events/ofEvents.h
            events/ofEventUtils.h
            communication/ofSerial.h
            communication/ofArduino.h
            ofMain.h
            graphics/ofImage.h
            graphics/ofTexture.h
            graphics/ofBitmapFont.h
            graphics/ofGraphics.h
            graphics/ofTrueTypeFont.h
            sound/ofSoundStream.h
            sound/ofSoundPlayer.h
            app/ofAppBaseWindow.h
            app/ofAppRunner.h
            app/ofAppGlutWindow.h
            app/ofBaseApp.h
            video/ofVideoPlayer.h
            video/ofQtUtils.h
            video/ofGstUtils.h
            video/ofVideoGrabber.h)


include_directories(${OPENFRAMEWORKS_INCLUDES})


add_library( openFrameworksStatic ${OFX_SOURCE_FILES})
set_target_properties (openFrameworksStatic 
                        PROPERTIES OUTPUT_NAME openFrameworks )
target_link_libraries(openFrameworksStatic ${OPENFRAMEWORKS_LIBRARIES})


add_library( openFrameworksShared SHARED ${OFX_SOURCE_FILES})
set_target_properties (openFrameworksShared 
                        PROPERTIES OUTPUT_NAME openFrameworks )
target_link_libraries(openFrameworksShared ${OPENFRAMEWORKS_LIBRARIES})




##################################################################### Packages and install 
## Package related 
#PKConfig
foreach (dir ${OPENFRAMEWORKS_INCLUDES})
    set(inccmd "${inccmd} -I${dir}")    
endforeach()

set ( prefix ${CMAKE_INSTALL_PREFIX})
set ( libdir ${CMAKE_INSTALL_PREFIX}/lib)
set ( includedir ${CMAKE_INSTALL_PREFIX}/include/openFrameworks )
set ( inccmd "${inccmd} -I${includedir}")
foreach (lib ${OPENFRAMEWORKS_LIBRARIES})
    if ( ${lib} MATCHES "-.+")
      set (libs "${libs} ${lib}" )
    else()
      set (libs "${libs} -l${lib}")
    endif()
endforeach()

set(flags "${inccmd}")

configure_file("openFrameworks.pc.in" "openFrameworks.pc")


install(FILES ${ofincludes_files}
    DESTINATION include/openFrameworks)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/openFrameworks.pc DESTINATION lib/pkgconfig)

install(TARGETS openFrameworksShared 
    DESTINATION lib)

install(TARGETS openFrameworksStatic 
    DESTINATION lib)

