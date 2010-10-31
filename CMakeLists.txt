CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

PROJECT(charsetdetect)

INCLUDE_DIRECTORIES(. nspr-emu mozilla/extensions/universalchardet/src/base/)

FILE(GLOB charsetdetect_lib_src mozilla/extensions/universalchardet/src/base/*.cpp)
SET(charsetdetect_lib_src ${charsetdetect_lib_src} charsetdetect.cpp)
ADD_LIBRARY(charsetdetect SHARED ${charsetdetect_lib_src})

# Do not output the shared library into the root
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

INSTALL (TARGETS charsetdetect DESTINATION lib)
INSTALL (FILES charsetdetect.h DESTINATION include)
