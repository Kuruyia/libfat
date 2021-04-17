cmake_minimum_required(VERSION 3.6)
add_definitions(-D__wiiu__)
set(CMAKE_TOOLCHAIN_FILE $ENV{WUT_ROOT}/share/wut.toolchain.cmake)

project(fat)

set(LIBFAT_MAJOR 1)
set(LIBFAT_MINOR 1)
set(LIBFAT_PATCH 0)

include("$ENV{WUT_ROOT}/share/wut.cmake" REQUIRED)

include_directories($ENV{DEVKITPRO}/portlibs/ppc/include
        $ENV{DEVKITPRO}/portlibs/wiiu/include)
link_directories($ENV{DEVKITPRO}/portlibs/ppc/lib
        $ENV{DEVKITPRO}/portlibs/wiiu/lib)

FILE(WRITE include/libfatversion.h
		"#ifndef __LIBFATVERSION_H__ \n"
		"#define __LIBFATVERSION_H__\n"
		"\n"
		"#define _LIBFAT_MAJOR_	 ${LIBFAT_MAJOR}\n"
		"#define _LIBFAT_MINOR_	 ${LIBFAT_MINOR}\n"
		"#define _LIBFAT_PATCH_	 ${LIBFAT_PATCH}\n"
		"\n"
		"#define _LIBFAT_STRING \"libFAT Release ${LIBFAT_MAJOR}.${LIBFAT_MINOR}.${LIBFAT_PATCH}\"\n"
		"\n"
		"#endif // __LIBFATVERSION_H__")

include_directories(include)

set(SOURCE_FILES
	source/cache.c
	source/directory.c
	source/disc.c
	source/fatdir.c
	source/fatfile.c
	source/file_allocation_table.c
	source/filetime.c
	source/libfat.c
	source/lock.c
	source/partition.c)
add_library(fat STATIC ${SOURCE_FILES})
target_link_libraries(fat iosuhax)