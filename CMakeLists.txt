cmake_minimum_required(VERSION 3.0.0)
project(robot-test VERSION 0.1.0)
set(CMAKE_CXX_STANDARD 17)

include(FetchContent)

#libraries.
FetchContent_Declare(
  replxx
  GIT_REPOSITORY    https://github.com/AmokHuginnsson/replxx.git
  GIT_TAG           release-0.0.4
  GIT_SHALLOW       1
)
FetchContent_MakeAvailable(replxx)



add_executable(toy-robot-game
  src/main.cpp
  src/table.cpp
  src/robot.cpp
  src/toy-robot-game.cpp
  src/repl-handler.cpp
  src/string-funcs.cpp
  )


# testing.

# GoogleTest requires at least C++11
FetchContent_Declare(
  googletest
  # URL https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
  GIT_REPOSITORY    https://github.com/google/googletest.git
  GIT_TAG           release-1.11.0
  GIT_SHALLOW       1
)
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

target_link_libraries(toy-robot-game PRIVATE 
  replxx
)

enable_testing()

add_executable(toy-robot-game-tests)

target_include_directories(toy-robot-game-tests PUBLIC "${PROJECT_SOURCE_DIR}/src")
target_sources(toy-robot-game-tests PRIVATE
  tests/main-test.cpp
  src/table.cpp
  tests/table-test.cpp
  src/robot.cpp
  tests/robot-test.cpp
  src/toy-robot-game.cpp
  tests/toy-robot-game-test.cpp
  src/string-funcs.cpp
  tests/string-funcs-test.cpp
)

target_link_libraries(toy-robot-game-tests PRIVATE 
  gmock_main
)

include(GoogleTest)
