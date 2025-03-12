find_package(PkgConfig REQUIRED)
pkg_check_modules(FUSE IMPORTED_TARGET fuse)

add_executable(fuse fuse.cpp)
target_link_libraries(fuse befs fs_shell root ${FUSE_LINK_LIBRARIES})

message(${FUSE_LINK_LIBRARIES})
message(${FUSE_INCLUDE_DIRS})

target_include_directories(fuse
	PRIVATE	
	${CMAKE_SOURCE_DIR}/src/fs_shell
	${FUSE_INCLUDE_DIRS}
	${CMAKE_SOURCE_DIR}/headers/build
	${CMAKE_SOURCE_DIR}/headers/os/support
	${CMAKE_SOURCE_DIR}/headers/private/fs_shell
)


target_compile_definitions(fuse PRIVATE
	FS_SHELL=1 
	HAIKU_BUILD_COMPATIBILITY_H __STRICT_ANSI__
	-DHAIKU_HOST_PLATFORM_64_BIT 
	-DHAIKU_PACKAGING_ARCH="x86_64" 
	HAIKU_HOST_PLATFORM_FREEBSD
	HAIKU_HOST_USE_XATTR_REF
	-D_FILE_OFFSET_BITS=64 
)
