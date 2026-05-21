cmake_minimum_required(VERSION 3.16)
project(Tricky_Forest)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# ====================================================
# 🔧 غير المسار ده لمسار SFML عندك
set(SFML_DIR "C:/SFML-2.6.1/lib/cmake/SFML")
# ====================================================

find_package(SFML 2.5 COMPONENTS graphics audio window system REQUIRED)

add_executable(Tricky_Forest src/main.cpp)

target_link_libraries(Tricky_Forest
    sfml-graphics
    sfml-audio
    sfml-window
    sfml-system
)

# نسخ الـ assets والـ levels جنب الـ exe تلقائياً
add_custom_command(TARGET Tricky_Forest POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/assets $<TARGET_FILE_DIR:Tricky_Forest>/assets
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/levels $<TARGET_FILE_DIR:Tricky_Forest>/levels
    COMMENT "Copying assets and levels..."
)

# نسخ DLL ملفات SFML تلقائياً (Windows)
if(WIN32)
    add_custom_command(TARGET Tricky_Forest POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
            ${SFML_DIR}/../../../bin $<TARGET_FILE_DIR:Tricky_Forest>
        COMMENT "Copying SFML DLLs..."
    )
endif()
