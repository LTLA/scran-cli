cmake_minimum_required(VERSION 3.14)

project(scrancli
    VERSION 1.0.0
    DESCRIPTION "A command-line interface for scran"
    LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

# Loading the dependencies.
include(FetchContent)

FetchContent_Declare(
  libscran
  GIT_REPOSITORY https://github.com/LTLA/libscran
  GIT_TAG bc5876b4f22eb0379b254137667c48ea5a2b5e65
)

FetchContent_MakeAvailable(libscran)

FetchContent_Declare(
  cli11
  GIT_REPOSITORY https://github.com/CLIUtils/CLI11
  GIT_TAG 70f8072f9dd2292fd0b9f9e5f58e279f60483ed3 # i.e. v2.1.2
)

FetchContent_MakeAvailable(cli11)

FetchContent_Declare(
  qdtsne
  GIT_REPOSITORY https://github.com/LTLA/qdtsne
  GIT_TAG 73480b9784f970c9477edd8eec6f431d2e06e5cd
)
FetchContent_MakeAvailable(qdtsne)

FetchContent_Declare(
  umappp
  GIT_REPOSITORY https://github.com/LTLA/umappp
  GIT_TAG 46e169b8df80725abca76e8007840ec17c54c819
)
FetchContent_MakeAvailable(umappp)

# Building the application.
add_executable(scrancli 
    src/main.cpp
    src/mito.cpp
)

set_target_properties(scrancli PROPERTIES OUTPUT_NAME "scran")

target_link_libraries(scrancli scran qdtsne umappp CLI11::CLI11)

include(CheckIncludeFiles)
check_include_files(filesystem HAVE_CXX_FS)
if (NOT HAVE_CXX_FS) 
    target_link_libraries(scrancli stdc++fs)
endif()

find_package(ZLIB)
target_link_libraries(scrancli ZLIB::ZLIB)
target_compile_definitions(scrancli PRIVATE TATAMI_USE_ZLIB=1)
