cmake_minimum_required(VERSION 3.10)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
project("befs_fuse")
add_subdirectory(src/bfs)
add_subdirectory(src/fs_shell)
add_subdirectory(src/root)
add_subdirectory(src/befuse)


