set(CMAKE_TOOLCHAIN_FILE CMakeToolchain.txt)
project(optee C)

# Required cmake version
cmake_minimum_required(VERSION 2.8.12)

# global needed variables
set(APPLICATION_NAME ${PROJECT_NAME})

set(APPLICATION_VERSION_MAJOR "2")
set(APPLICATION_VERSION_MINOR "0")
set(APPLICATION_VERSION_PATCH "0")

set(APPLICATION_VERSION
	"${APPLICATION_VERSION_MAJOR}.${APPLICATION_VERSION_MINOR}.${APPLICATION_VERSION_PATCH}")

include_directories(tee-supplicant/src
	            public
		    libteec/include)

set(LIBTEE-SRC libteec/src/tee_client_api.c
	       libteec/src/teec_trace.c)
add_library(tee SHARED ${LIBTEE-SRC})

set(TS-SRC tee-supplicant/src/handle.c
	tee-supplicant/src/hmac_sha2.c
	tee-supplicant/src/rpmb.c
	tee-supplicant/src/sha2.c
	tee-supplicant/src/tee_supp_fs.c
	tee-supplicant/src/tee_supplicant.c
	tee-supplicant/src/teec_ta_load.c)

add_definitions(-DBINARY_PREFIX="TEES")
add_executable(tee-supplicant ${TS-SRC})
target_link_libraries(tee-supplicant tee)


