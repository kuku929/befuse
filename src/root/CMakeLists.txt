SET(ROOT_SOURCES
	atomic.cpp
	byteorder.cpp
	errors.cpp
	find_directory.cpp
	fs.cpp
	fs_attr.cpp
	fs_descriptors.cpp
	misc.cpp
	sem.cpp
	thread.cpp
	SHA256.cpp

	fs_freebsd.cpp

	driver_settings.cpp

	strlcpy.c
	strlcat.c
	strnlen.c

	KMessage.cpp
)
add_library(root ${ROOT_SOURCES})
target_compile_options(root PRIVATE 
	-O1 -Wno-multichar 
	-fms-extensions -frtti 
	-Wno-pointer-to-int-cast
	PRIVATE "-include" "${CMAKE_SOURCE_DIR}/headers/build/BeOSBuildCompatibility.h")
target_include_directories(root
	PUBLIC 
	${CMAKE_SOURCE_DIR}/headers/private/fs_shell
	${CMAKE_SOURCE_DIR}/headers/private/libroot
	${CMAKE_SOURCE_DIR}/headers/build
	${CMAKE_SOURCE_DIR}/headers/build/private/system
	${CMAKE_SOURCE_DIR}/headers/build/private/kernel
	${CMAKE_SOURCE_DIR}/headers/build/os
	${CMAKE_SOURCE_DIR}/headers/build/os/interface
	${CMAKE_SOURCE_DIR}/headers/build/os/app
	${CMAKE_SOURCE_DIR}/headers/build/os/drivers
	${CMAKE_SOURCE_DIR}/headers/build/os/storage
	${CMAKE_SOURCE_DIR}/headers/build/os/support
	${CMAKE_SOURCE_DIR}/headers/build/os/kernel
)
set_source_files_properties(errors.cpp PROPERTIES COMPILE_DEFINITIONS BUILDING_HAIKU_ERROR_MAPPER=1)
set_source_files_properties(KMessage.cpp PROPERTIES COMPILE_DEFINITIONS KMESSAGE_CONTAINER_ONLY=1)
target_compile_definitions(root PRIVATE
	_HAIKU_BUILD_DONT_REMAP_FD_FUNCTIONS=1
	HAIKU_HOST_USE_XATTR
	HAIKU_HOST_USE_XATTR_REF
	-D_FILE_OFFSET_BITS_=64
	HAIKU_HOST_PLATFORM_FREEBSD
	HAIKU_BUILD_GENERATED_DIRECTORY="${CMAKE_CURRENT_BINARY_DIR}"
	HAIKU_BUILD_ATTRIBUTES_DIR="${CMAKE_CURRENT_BINARY_DIR}/attributes"
)

add_library(function_remapper function_remapper.cpp)

target_compile_options(function_remapper PRIVATE 
	-O1 -Wno-multichar 
	-fms-extensions -frtti 
	-Wno-pointer-to-int-cast
	PRIVATE "-include" "${CMAKE_SOURCE_DIR}/headers/build/BeOSBuildCompatibility.h"
	-fPIC)

target_include_directories(function_remapper
	PRIVATE	
	${CMAKE_SOURCE_DIR}/headers/private/fs_shell
	${CMAKE_SOURCE_DIR}/headers/private/system
	${CMAKE_SOURCE_DIR}/headers/private/libroot
	${CMAKE_SOURCE_DIR}/headers/build
	${CMAKE_SOURCE_DIR}/headers/build/private/system
	${CMAKE_SOURCE_DIR}/headers/build/private/kernel
	${CMAKE_SOURCE_DIR}/headers/build/os
	${CMAKE_SOURCE_DIR}/headers/build/os/interface
	${CMAKE_SOURCE_DIR}/headers/build/os/app
	${CMAKE_SOURCE_DIR}/headers/build/os/drivers
	${CMAKE_SOURCE_DIR}/headers/build/os/storage
	${CMAKE_SOURCE_DIR}/headers/build/os/support
	${CMAKE_SOURCE_DIR}/headers/build/os/kernel
)

target_compile_definitions(function_remapper PRIVATE
	_HAIKU_BUILD_DONT_REMAP_FD_FUNCTIONS=1
	-DHAIKU_HOST_PLATFORM_64_BIT 
	-DHAIKU_PACKAGING_ARCH="x86_64" 
	HAIKU_HOST_USE_XATTR_REF
	-D_FILE_OFFSET_BITS_=64
	HAIKU_HOST_PLATFORM_FREEBSD
	HAIKU_BUILD_GENERATED_DIRECTORY="${CMAKE_CURRENT_BINARY_DIR}"
	HAIKU_BUILD_ATTRIBUTES_DIR="${CMAKE_CURRENT_BINARY_DIR}/attributes"
)

