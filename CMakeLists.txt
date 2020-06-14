cmake_minimum_required(VERSION 3.2)
project(wiiu-fw-emulator LANGUAGES CXX)
set(CXX_STANDARD 14)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake OPTIONAL RESULT_VARIABLE USING_CONAN)
if(USING_CONAN)
	conan_basic_setup(NO_OUTPUT_DIRS)
endif()

include(CheckCXXCompilerFlag)
check_cxx_compiler_flag(-Wno-invalid-offsetof HAS_NO_INVALID_OFFSETOF)
if(HAS_NO_INVALID_OFFSETOF)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-invalid-offsetof")
endif()

check_cxx_compiler_flag(-fno-stack-protector HAS_NO_INVALID_STACK_PROTECTOR)
if(HAS_NO_INVALID_STACK_PROTECTOR)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-stack-protector")
endif()

find_package(OpenSSL REQUIRED)
find_package(Threads REQUIRED)

file(GLOB_RECURSE SOURCE_FILES src/*.cpp)
file(GLOB_RECURSE HEADER_FILES src/*.h)

add_executable(wiiu-fw-emulator
	${SOURCE_FILES}
	${HEADER_FILES})
target_link_libraries(wiiu-fw-emulator
	OpenSSL::SSL
	Threads::Threads)
target_include_directories(wiiu-fw-emulator
	PRIVATE src)
