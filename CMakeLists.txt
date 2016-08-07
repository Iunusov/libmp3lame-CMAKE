cmake_minimum_required(VERSION 3.0)

project (libmp3lame)

if(WIN32)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/Dll)
endif()

add_definitions(-DHAVE_STDINT_H)
add_definitions(-DHAVE_MPGLIB)

if((${CMAKE_CXX_COMPILER_ID} STREQUAL MSVC))
add_definitions(-DUSE_LAYER_2)
add_definitions(-DTAKEHIRO_IEEE754_HACK)
add_definitions(-DHAVE_CONFIG_H)
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_CRT_SECURE_NO_DEPRECATED)
configure_file(configMS.h config.h @ONLY)
else()
add_definitions(-DSTDC_HEADERS)
if(NOT ieee754_float32_t)
add_definitions(-Dieee754_float32_t=float)
endif()
endif()

set(SOURCE_LIB
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/bitstream.c 
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/encoder.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/fft.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/gain_analysis.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/newmdct.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/lame.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/id3tag.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/mpglib_interface.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/presets.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/psymodel.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/quantize.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/quantize_pvt.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/reservoir.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/set_get.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/tables.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/takehiro.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/util.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/vbrquantize.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/VbrTag.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/version.c
${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame/vector/xmm_quantize_sub.c
${CMAKE_CURRENT_SOURCE_DIR}/mpglib/common.c
${CMAKE_CURRENT_SOURCE_DIR}/mpglib/dct64_i386.c
${CMAKE_CURRENT_SOURCE_DIR}/mpglib/decode_i386.c
${CMAKE_CURRENT_SOURCE_DIR}/mpglib/interface.c
${CMAKE_CURRENT_SOURCE_DIR}/mpglib/layer1.c
${CMAKE_CURRENT_SOURCE_DIR}/mpglib/layer2.c
${CMAKE_CURRENT_SOURCE_DIR}/mpglib/layer3.c
${CMAKE_CURRENT_SOURCE_DIR}/mpglib/tabinit.c)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)


include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/libmp3lame)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/mpglib)

include_directories(${CMAKE_CURRENT_BINARY_DIR})

add_library(${PROJECT_NAME} STATIC ${SOURCE_LIB})

target_include_directories(${PROJECT_NAME} 
PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
)

set_property(TARGET ${PROJECT_NAME} PROPERTY POSITION_INDEPENDENT_CODE ON)