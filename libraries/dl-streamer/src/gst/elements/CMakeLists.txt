# ==============================================================================
# Copyright (C) 2022-2025 Intel Corporation
#
# SPDX-License-Identifier: MIT
# ==============================================================================

set(TARGET_NAME "gstdlstreamer_elements")

pkg_search_module(VA va libva REQUIRED)

add_subdirectory(batch_create)
add_subdirectory(batch_split)
add_subdirectory(capsrelax)
add_subdirectory(gvadrop)
add_subdirectory(gvafpscounter)
add_subdirectory(meta_aggregate)
add_subdirectory(roi_split)
add_subdirectory(video_frames_buffer)
add_subdirectory(meta_smooth)
add_subdirectory(gvainference)

file(GLOB MAIN_SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/batch_create/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/batch_split/*.cpp
)

file(GLOB MAIN_HEADERS
    ${CMAKE_CURRENT_SOURCE_DIR}/*.h
)

add_library(${TARGET_NAME} SHARED ${MAIN_SRC} ${MAIN_HEADERS})
set_compile_flags(${TARGET_NAME})

target_link_directories(${TARGET_NAME} PUBLIC ${VA_LIBRARY_DIRS})

target_link_libraries(${TARGET_NAME}
PRIVATE
    batch_create
    batch_split
    capsrelax
    gvadrop
    gvafpscounter
    meta_aggregate
    roi_split
    meta_smooth
    video_frames_buffer
    dlstreamer_gst
    gstvideoanalyticsmeta
    gvainference
)

install(TARGETS ${TARGET_NAME} DESTINATION ${DLSTREAMER_PLUGINS_INSTALL_PATH})
