project(libremovefile)

cmake_minimum_required(VERSION 2.4.0)

include_directories(${DARLING_TOP_DIRECTORY}/platform-include)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/gen)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_SOURCE_DIR}/src/libc/darwin)
include_directories(${CMAKE_SOURCE_DIR}/src/libc/stdtime/FreeBSD)
include_directories(${CMAKE_SOURCE_DIR}/src/libc/locale/FreeBSD)
include_directories(${CMAKE_SOURCE_DIR}/src/libc/locale)
include_directories(${CMAKE_SOURCE_DIR}/src/kernel/libsyscall/wrappers)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fblocks")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -nostdlib")

add_definitions(-w -nostdinc)

set(removefile_sources
	removefile.c
	removefile_random.c
	removefile_rename_unlink.c
	removefile_sunlink.c
	removefile_tree_walker.c
)

set(DYLIB_INSTALL_NAME "/usr/lib/system/libremovefile.dylib")
add_darling_library(removefile SHARED ${removefile_sources})
target_link_libraries(removefile system_c system_kernel system_dyld system_malloc compiler_rt)
make_fat(removefile)

install(TARGETS removefile DESTINATION libexec/darling/usr/lib/system)

