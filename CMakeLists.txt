cmake_minimum_required(VERSION 3.15)

project(ChezScheme)

set( CMAKE_VERBOSE_MAKEFILE on )

add_compile_options(-m64 -Wpointer-arith -Wall -Wextra -Wno-implicit-fallthrough -Werror -O0 -DX86_64)

add_library(kernel OBJECT statics.c segment.c alloc.c symbol.c intern.c gcwrapper.c gc-ocd.c gc-oce.c number.c schsig.c io.c new-io.c print.c fasl.c stats.c foreign.c prim.c prim5.c flushcache.c schlib.c thread.c expeditor.c scheme.c compress-io.c i3le.c)
target_include_directories(kernel
        PRIVATE boot/a6osx zlib lz4/lib /opt/X11/include/)

add_executable(scheme main.c)
target_include_directories(scheme
        PRIVATE boot/a6osx /opt/X11/include/)
target_link_libraries(scheme
        kernel
        ${CMAKE_SOURCE_DIR}/lz4/lib/liblz4.a ${CMAKE_SOURCE_DIR}/zlib/libz.a
        /usr/lib/libiconv.dylib /usr/lib/libm.dylib /usr/lib/libncurses.dylib)
