cmake_minimum_required(VERSION 3.1)

# Set project
set(PROJECTNAME Mine-imator)
project(${PROJECTNAME})

# Add files
file(GLOB_RECURSE SRCS "${CMAKE_SOURCE_DIR}/src/*.cpp")
file(GLOB_RECURSE HDRS "${CMAKE_SOURCE_DIR}/src/*.h")
add_executable(${PROJECTNAME} ${HDRS} ${SRCS})

# Add GLFW
add_subdirectory("dep/glfw")
include_directories("dep/glfw/include")
set(LIBS glfw ${GLFW_LIBRARIES})

# Add GLEW
include_directories("dep/glew")
file(GLOB GLEW_SRCS "${CMAKE_SOURCE_DIR}/dep/glew/*.c")
add_library(glew STATIC ${GLEW_SRCS})
set(LIBS ${LIBS} glew)

# Add dlib
include_directories("dep/dlib")
file(GLOB DLIB_SRCS "${CMAKE_SOURCE_DIR}/dep/dlib/*.cpp")
add_library(dlib STATIC ${DLIB_SRCS})
set(LIBS ${LIBS} dlib)

# Add SOIL
include_directories("dep/soil")
file(GLOB SOIL_SRCS "${CMAKE_SOURCE_DIR}/dep/soil/*.c")
add_library(soil STATIC ${SOIL_SRCS})
set(LIBS ${LIBS} soil)

# Add zlib
find_package(ZLIB REQUIRED)
message("ZLIB_FOUND = ${ZLIB_FOUND}")
include_directories(${ZLIB_INCLUDE_DIR})
set(LIBS ${LIBS} ${ZLIB_LIBRARIES})

# Add minizip
include_directories("dep/minizip")
file(GLOB MINIZIP_SRCS "${CMAKE_SOURCE_DIR}/dep/minizip/*.c")
add_library(minizip STATIC ${MINIZIP_SRCS})
target_link_libraries(minizip ${ZLIB_LIBRARIES})
set(LIBS ${LIBS} minizip)

# Finally, link the libraries
message("LIBS = ${LIBS}")
target_link_libraries(${PROJECTNAME} ${LIBS})