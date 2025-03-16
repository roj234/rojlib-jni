cmake_minimum_required(VERSION 3.23)
project(cpp)

include_directories("$ENV{JAVA_HOME}\\include")
include_directories("$ENV{JAVA_HOME}\\include\\win32")
include_directories("${CMAKE_CURRENT_BINARY_DIR}/libdivsufsort/include")

if(MSVC)
    add_compile_options("/O2" "/arch:SSE4.1" "/D__SSE4_1__" "/D__AES__")
    add_library(cpp SHARED main.cpp)
    target_link_libraries(cpp PRIVATE divsufsort PRIVATE winpthread PRIVATE ws2_32 PRIVATE bcrypt)
else()
    add_compile_options("-O2" "-maes" "-msse4" "-pipe")
    add_library(cpp SHARED main.cpp)
    target_link_libraries(cpp PRIVATE divsufsort PRIVATE gcc PRIVATE stdc++ PRIVATE winpthread PRIVATE ws2_32 PRIVATE bcrypt)
endif()

function(add_libdivsufsort)
    set(CMAKE_POLICY_DEFAULT_CMP0077 NEW)
    set(BUILD_EXAMPLES OFF)
    set(BUILD_SHARED_LIBS OFF)
    add_subdirectory(libdivsufsort)
endfunction()
add_libdivsufsort()

install(
        TARGETS cpp
        RUNTIME DESTINATION ${PROJECT_BINARY_DIR}/bin
        LIBRARY DESTINATION ${PROJECT_BINARY_DIR}/bin
)