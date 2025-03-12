cmake_minimum_required(VERSION 3.10)
project("befs_fuse")
#add_compile_options("-H")
add_subdirectory(src/bfs)
add_subdirectory(src/fs_shell)
add_subdirectory(src/root)
add_subdirectory(src/fuse)


