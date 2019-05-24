cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(
  HUNTER_CACHE_SERVERS
  "https://github.com/bmstu-iu8-cpp-sem-3/hunter-cache"
  CACHE STRING "Default cache server"
)

include("tools/gate/cmake/HunterGate.cmake")

huntergate(
  URL "https://github.com/ruslo/hunter/archive/v0.23.34.tar.gz"
  SHA1 "70287b1ffa810ee4e952052a9adff9b4856d0d54"
)

project(banking)

hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)

add_library(banking
    STATIC Account.cpp Transaction.cpp
)

add_executable(tests
    ${CMAKE_CURRENT_SOURCE_DIR}/test.cpp
)

target_include_directories(banking
   PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
)

target_include_directories(tests
   PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
)


target_link_libraries(tests GTest::main banking)

enable_testing()
add_test(NAME unit_tests COMMAND tests) 