find_package(PkgConfig REQUIRED)
pkg_check_modules(FUSE IMPORTED_TARGET fuse3)

add_executable(befuse fuse.cpp)
target_link_libraries(befuse befs fs_shell root ${FUSE_LINK_LIBRARIES})

message(${FUSE_LINK_LIBRARIES})
message(${FUSE_INCLUDE_DIRS})

target_include_directories(befuse
	PRIVATE	
	${CMAKE_SOURCE_DIR}/src/fs_shell
	${FUSE_INCLUDE_DIRS}
	${CMAKE_SOURCE_DIR}/headers/build
	${CMAKE_SOURCE_DIR}/headers/os/support
	${CMAKE_SOURCE_DIR}/headers/private/fs_shell
)


target_compile_definitions(befuse PRIVATE
	FS_SHELL=1 
	HAIKU_BUILD_COMPATIBILITY_H __STRICT_ANSI__
	-DHAIKU_HOST_PLATFORM_64_BIT 
	-DHAIKU_PACKAGING_ARCH="x86_64" 
	HAIKU_HOST_PLATFORM_FREEBSD
	HAIKU_HOST_USE_XATTR_REF
	-D_FILE_OFFSET_BITS=64 
)
install(TARGETS befuse
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
    RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
)
