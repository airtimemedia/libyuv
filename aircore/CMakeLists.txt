# Copyright Airtime Media 2014

set(YUV_SOURCES
  # headers
  ${at_media_deps_dir}/libyuv/include/libyuv/basic_types.h
  ${at_media_deps_dir}/libyuv/include/libyuv/compare.h
  ${at_media_deps_dir}/libyuv/include/libyuv/compare_row.h
  ${at_media_deps_dir}/libyuv/include/libyuv/convert.h
  ${at_media_deps_dir}/libyuv/include/libyuv/convert_argb.h
  ${at_media_deps_dir}/libyuv/include/libyuv/convert_from.h
  ${at_media_deps_dir}/libyuv/include/libyuv/convert_from_argb.h
  ${at_media_deps_dir}/libyuv/include/libyuv/cpu_id.h
  ${at_media_deps_dir}/libyuv/include/libyuv/mjpeg_decoder.h
  ${at_media_deps_dir}/libyuv/include/libyuv/planar_functions.h
  ${at_media_deps_dir}/libyuv/include/libyuv/rotate.h
  ${at_media_deps_dir}/libyuv/include/libyuv/rotate_argb.h
  ${at_media_deps_dir}/libyuv/include/libyuv/rotate_row.h
  ${at_media_deps_dir}/libyuv/include/libyuv/row.h
  ${at_media_deps_dir}/libyuv/include/libyuv/scale.h
  ${at_media_deps_dir}/libyuv/include/libyuv/scale_argb.h
  ${at_media_deps_dir}/libyuv/include/libyuv/scale_row.h
  ${at_media_deps_dir}/libyuv/include/libyuv/version.h
  ${at_media_deps_dir}/libyuv/include/libyuv/video_common.h
  ${at_media_deps_dir}/libyuv/include/libyuv.h
  ${at_media_deps_dir}/libyuv/util/psnr.h
  ${at_media_deps_dir}/libyuv/util/ssim.h

  # sources
  ${at_media_deps_dir}/libyuv/source/compare.cc
  ${at_media_deps_dir}/libyuv/source/compare_common.cc
  ${at_media_deps_dir}/libyuv/source/compare_gcc.cc
  ${at_media_deps_dir}/libyuv/source/convert.cc
  ${at_media_deps_dir}/libyuv/source/convert_argb.cc
  ${at_media_deps_dir}/libyuv/source/convert_from.cc
  ${at_media_deps_dir}/libyuv/source/convert_from_argb.cc
  ${at_media_deps_dir}/libyuv/source/convert_jpeg.cc
  ${at_media_deps_dir}/libyuv/source/convert_to_argb.cc
  ${at_media_deps_dir}/libyuv/source/convert_to_i420.cc
  ${at_media_deps_dir}/libyuv/source/cpu_id.cc
  ${at_media_deps_dir}/libyuv/source/planar_functions.cc
  ${at_media_deps_dir}/libyuv/source/rotate.cc
  ${at_media_deps_dir}/libyuv/source/rotate_any.cc
  ${at_media_deps_dir}/libyuv/source/rotate_argb.cc
  ${at_media_deps_dir}/libyuv/source/rotate_common.cc
  ${at_media_deps_dir}/libyuv/source/rotate_gcc.cc
  ${at_media_deps_dir}/libyuv/source/row_any.cc
  ${at_media_deps_dir}/libyuv/source/row_common.cc
  ${at_media_deps_dir}/libyuv/source/row_gcc.cc
  ${at_media_deps_dir}/libyuv/source/scale.cc
  ${at_media_deps_dir}/libyuv/source/scale_any.cc
  ${at_media_deps_dir}/libyuv/source/scale_argb.cc
  ${at_media_deps_dir}/libyuv/source/scale_common.cc
  ${at_media_deps_dir}/libyuv/source/scale_gcc.cc
  ${at_media_deps_dir}/libyuv/source/video_common.cc
  ${at_media_deps_dir}/libyuv/util/compare.cc
  ${at_media_deps_dir}/libyuv/util/psnr.cc
  ${at_media_deps_dir}/libyuv/util/psnr_main.cc
  ${at_media_deps_dir}/libyuv/util/ssim.cc
  ${at_media_deps_dir}/libyuv/util/yuvconvert.cc
)

if(YUV_HAVE_JPEG)
  list(APPEND YUV_SOURCES
    ${at_media_deps_dir}/libyuv/source/convert_jpeg.cc
    ${at_media_deps_dir}/libyuv/source/mjpeg_decoder.cc
    ${at_media_deps_dir}/libyuv/source/mjpeg_validate.cc
  )
  add_definitions(-DHAVE_JPEG)
endif()

if(${at_target_os} MATCHES windows)
  list(APPEND YUV_SOURCES
    ${at_media_deps_dir}/libyuv/source/compare_win.cc
    ${at_media_deps_dir}/libyuv/source/rotate_win.cc
    ${at_media_deps_dir}/libyuv/source/row_win.cc
    ${at_media_deps_dir}/libyuv/source/scale_win.cc
  )
endif()

if(${at_target_arch} MATCHES armv7 OR ${at_target_arch} MATCHES arm64 OR IOS)
  list(APPEND YUV_SOURCES
    ${at_media_deps_dir}/libyuv/source/compare_neon.cc
    ${at_media_deps_dir}/libyuv/source/compare_neon64.cc
    ${at_media_deps_dir}/libyuv/source/rotate_neon.cc
    ${at_media_deps_dir}/libyuv/source/rotate_neon64.cc
    ${at_media_deps_dir}/libyuv/source/row_neon.cc
    ${at_media_deps_dir}/libyuv/source/row_neon64.cc
    ${at_media_deps_dir}/libyuv/source/scale_neon.cc
    ${at_media_deps_dir}/libyuv/source/scale_neon64.cc
  )
elseif(${at_target_arch} MATCHES mips)
  list(APPEND YUV_SOURCES
    ${at_media_deps_dir}/libyuv/include/libyuv/macros_msa.h
    ${at_media_deps_dir}/libyuv/source/compare_msa.cc
    ${at_media_deps_dir}/libyuv/source/rotate_msa.cc
    ${at_media_deps_dir}/libyuv/source/row_msa.cc
    ${at_media_deps_dir}/libyuv/source/scale_msa.cc
  )
endif()

set_source_files_properties(${YUV_SOURCES}
  PROPERTIES COMPILE_FLAGS "${CMAKE_CXX_FLAGS} -Wno-asm-operand-widths -Wno-unused-variable"
)

set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -O2")

add_library(at_libyuv STATIC ${YUV_SOURCES})
set_target_properties(at_libyuv PROPERTIES
  OUTPUT_NAME yuv
)

target_include_directories(at_libyuv
  PUBLIC ${at_media_deps_dir}/libyuv/include

# NOTE(tom): we can't use target_include_directories on an imported library,
# so we directly set the properties that match the behavior we want (PUBLIC
# scope)
#
# See https://cmake.org/cmake/help/v3.0/command/target_include_directories.html
# and http://stackoverflow.com/a/28305481/2442139
set_property(TARGET at_libyuv APPEND PROPERTY INTERFACE_INCLUDE_DIRECTORIES
  ${at_media_deps_dir}/libyuv/include
)
)