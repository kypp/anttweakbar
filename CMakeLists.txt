Project(AntTweakBar)
cmake_minimum_required(VERSION 2.8)

if (BUILD_SHARED_LIBS)
	add_definitions(-DTW_EXPORTS)
else (BUILD_SHARED_LIBS)
	add_definitions(-DTW_STATIC)
endif (BUILD_SHARED_LIBS)

# don't use pragma comment(lib)
add_definitions(-DTW_NO_LIB_PRAGMA)

include_directories (include)

add_subdirectory(src)

install (DIRECTORY include/ DESTINATION include)