cmake_minimum_required(VERSION 3.0.0 FATAL_ERROR)

project(p2p_pg_external)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ../build)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ../build)

set(dht_target "dht")
set(dht_srcs dht/dht.c)
add_library(${dht_target} STATIC ${dht_srcs})
target_include_directories(${dht_target} PUBLIC dht/)

set(pittacus_target "pittacus")
set(pittacus_srcs pittacus/src/gossip.c pittacus/src/member.c pittacus/src/messages.c pittacus/src/network.c pittacus/src/utils.c pittacus/src/vector_clock.c)
add_library(${pittacus_target} STATIC ${pittacus_srcs})
target_include_directories(${pittacus_target} PUBLIC pittacus/src/)

set(raft "raft")
set(raft_srcs raft/src/raft_server.c raft/src/raft_server_properties.c raft/src/raft_log.c raft/src/raft_node.c)
add_library(${raft} STATIC ${raft_srcs})
target_include_directories(${raft} PUBLIC raft/include/)

set(sqlite3 "sqlite3")
set(sqlite_srcs sqlite/sqlite3.c)
add_library(${sqlite3} STATIC ${sqlite_srcs})
target_include_directories(${sqlite3} PUBLIC sqlite/)
target_compile_definitions(${sqlite3} PUBLIC SQLITE_ENABLE_FTS5)

set(linenoise_target "linenoise")
set(linenoise_srcs linenoise/linenoise.c)
add_library(${linenoise_target} STATIC ${linenoise_srcs})
target_include_directories(${linenoise_target} PUBLIC linenoise/)

#[[set(_target "")
set(_srcs )
add_library(${} STATIC ${_srcs})
target_include_directories(${} PUBLIC )
]]
