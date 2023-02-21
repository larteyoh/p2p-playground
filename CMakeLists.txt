cmake_minimum_required(VERSION 3.5)
project(p2p_playground)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(VERSION_MAJOR "0")
set(VERSION_MINOR "1")
set(VERSION_PATCH "0")
set(PROJECT_VERSION ${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH})

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
######################################
# dht
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/dht)
set(dht_srcs ${CMAKE_CURRENT_SOURCE_DIR}/build/libdht.a)

######################################
# pittacus
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/pittacus/src)
set(pittacus_srcs ${CMAKE_CURRENT_SOURCE_DIR}/build/libpittacus.a)

######################################
# raft
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/raft/include)
set(raft_srcs ${CMAKE_CURRENT_SOURCE_DIR}/build/libraft.a)

######################################
# sqlite3
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/sqlite) # v3.39.1 (amalgamation - said to run a bit faster)
set(sqlite_srcs ${CMAKE_CURRENT_SOURCE_DIR}/build/libsqlite3.a)

######################################
# libuv
find_package(LibUV)
if(LIBUV_FOUND)
    message(STATUS "Using LibUV: ${LibUV_LIBRARIES} (v${LibUV_VERSION})")
    include_directories(${LibUV_INCLUDE_DIRS})
    set(libuv_srcs ${LibUV_LIBRARIES})
endif()
if(NOT LIBUV_FOUND)
    include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/libuv/include ${CMAKE_CURRENT_SOURCE_DIR}/external/libuv/src)
    set(libuv_srcs ${CMAKE_CURRENT_SOURCE_DIR}/external/libuv/.libs/libuv.a) # In case user chooses to build libuv themselves instead of installing it on the system
endif()

######################################
# linenoise - will act as client sending requests to the server
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/linenoise/)
set(linenoise_srcs ${CMAKE_CURRENT_SOURCE_DIR}/build/liblinenoise.a)

######################################
link_directories(${CMAKE_CURRENT_SOURCE_DIR}/build)
set(p2p_pg_sources ${CMAKE_CURRENT_SOURCE_DIR}/src/dht.cpp ${CMAKE_CURRENT_SOURCE_DIR}/src/gossip.cpp ${CMAKE_CURRENT_SOURCE_DIR}/src/consensus.cpp ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp)
set(p2p_pg_executable "p2p_pg")
add_executable(${p2p_pg_executable} ${p2p_pg_sources})
target_include_directories(${p2p_pg_executable} PRIVATE #[[${CMAKE_CURRENT_SOURCE_DIR}/src/]])
target_link_libraries(${p2p_pg_executable} ${dht_srcs} ${pittacus_srcs} ${raft_srcs} ${sqlite_srcs} ${libuv_srcs} ${linenoise_srcs})

######################################
if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    #[[set(posix_src libpthread.so libdl.so libm.so)
    target_link_libraries(${target_executable} ${posix_src})]]
endif()

if(WIN32)
    #[[set(winsock2_src ws2_32.lib)
    target_link_libraries(${target_executable} ${winsock2_src})]]
endif()

######################################
