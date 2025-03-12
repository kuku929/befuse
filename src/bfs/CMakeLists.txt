SET(BEFS_SOURCES 	
	bfs_disk_system.cpp
	BlockAllocator.cpp
	BPlusTree.cpp
	Attribute.cpp
	CheckVisitor.cpp
	Debug.cpp
	DeviceOpener.cpp
	FileSystemVisitor.cpp
	Index.cpp Inode.cpp
	Journal.cpp
	Query.cpp
	QueryParserUtils.cpp
	ResizeVisitor.cpp
	Volume.cpp

	kernel_interface.cpp
)
add_library(befs ${BEFS_SOURCES})
message(STATUS "CMAKE_SOURCE_DIR: ${CMAKE_SOURCE_DIR}")
target_compile_options(befs PRIVATE -std=c++11 -O1 -Wno-multichar -fms-extensions -fno-rtti)
target_include_directories(befs 
	PRIVATE	
	${CMAKE_SOURCE_DIR}/headers/
	${CMAKE_SOURCE_DIR}/headers/os
	${CMAKE_SOURCE_DIR}/headers/build/os/support
	${CMAKE_SOURCE_DIR}/headers/build/os
	${CMAKE_SOURCE_DIR}/headers/private
	${CMAKE_SOURCE_DIR}/headers/private/fs_shell
		)
target_compile_definitions(befs PRIVATE FS_SHELL
	HAIKU_HOST_USE_XATTR_REF
	-DHAIKU_HOST_PLATFORM_64_BIT 
	-DHAIKU_PACKAGING_ARCH="x86_64" 
	-D_FILE_OFFSET_BITS=64)
