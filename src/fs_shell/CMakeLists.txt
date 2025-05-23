SET(FSSHELL_SOURCES 	
	atomic.cpp
	block_cache.cpp
	byte_order.cpp
	command_cp.cpp
	disk_device_manager.cpp
	driver_settings.cpp
	errno.cpp
	fcntl.cpp
	fd.cpp
	file_cache.cpp
	file_map.cpp
	kernel_export.cpp
	KPath.cpp
	hash.cpp
	list.cpp
	lock.cpp
	module.cpp
	node_monitor.cpp
	partition_support.cpp
	path_util.cpp
	rootfs.cpp
	sem.cpp
	stat.cpp
	stat_util.cpp
	stdio.cpp
	string.cpp
	thread.cpp
	time.cpp
	uio.cpp
	unistd.cpp
	vfs.cpp
	external_commands_unix.cpp
	fs_shell_command_unix.cpp 
)
add_library(fs_shell ${FSSHELL_SOURCES})
target_compile_options(fs_shell PRIVATE -std=c++11 -O1 -Wno-multichar -fms-extensions -fno-rtti)
target_include_directories(fs_shell
	PRIVATE	
	${CMAKE_SOURCE_DIR}/headers
	${CMAKE_SOURCE_DIR}/headers/build
	${CMAKE_SOURCE_DIR}/headers/private/fs_shell
	${CMAKE_SOURCE_DIR}/headers/private/shared
	${CMAKE_SOURCE_DIR}/headers/private/file_systems
	${CMAKE_SOURCE_DIR}/headers/build/os
	${CMAKE_SOURCE_DIR}/headers/build/os/storage
	${CMAKE_SOURCE_DIR}/headers/build/os/support
	${CMAKE_SOURCE_DIR}/headers/build/os/kernel
	${CMAKE_SOURCE_DIR}/headers/private/userlandfs
)
target_compile_definitions(fs_shell PRIVATE
	FS_SHELL=1 
	HAIKU_BUILD_COMPATIBILITY_H __STRICT_ANSI__
	-DHAIKU_HOST_PLATFORM_64_BIT 
	-DHAIKU_PACKAGING_ARCH="x86_64" 
	HAIKU_HOST_USE_XATTR_REF
	HAIKU_HOST_PLATFORM_FREEBSD
	-D_FILE_OFFSET_BITS=64 
)
