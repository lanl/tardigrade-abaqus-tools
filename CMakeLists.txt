cmake_minimum_required (VERSION 3.14)

project (cpp_stub VERSION 0.0.1)

# Configure version
configure_file("src/cpp/config.h.in" "${PROJECT_BINARY_DIR}/config.h")

# Find eigen
set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} $ENV{HOME}/.local $ENV{HOME}/local/ $ENV{HOME})
find_package (Eigen3 3.3 REQUIRED NO_MODULE)
include_directories(${EIGEN3_INCLUDE_DIR})

# Add the cmake folder to locate the FindSphinx module
set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})

# Find related, but external, projects
# TODO: Decide how to handle master/dev build differences. bash env variables for GIT TAG?
include(FetchContent)
FetchContent_Declare(
    error_tools
    GIT_REPOSITORY ssh://git@xcp-stash.lanl.gov:7999/mm/error_tools.git
    GIT_TAG        origin/dev
)
FetchContent_Declare(
    vector_tools
    GIT_REPOSITORY ssh://git@xcp-stash.lanl.gov:7999/mm/vector_tools.git
    GIT_TAG        origin/dev
)
FetchContent_Declare(
    constitutive_tools
    GIT_REPOSITORY ssh://git@xcp-stash.lanl.gov:7999/mm/constitutive_tools.git
    GIT_TAG        origin/dev
)
FetchContent_Declare(
    stress_tools
    GIT_REPOSITORY ssh://git@xcp-stash.lanl.gov:7999/mm/stress_tools.git
    GIT_TAG        origin/dev
)
FetchContent_Declare(
    solver_tools
    GIT_REPOSITORY ssh://git@xcp-stash.lanl.gov:7999/mm/solver_tools.git
    GIT_TAG        origin/dev
)
FetchContent_MakeAvailable(error_tools vector_tools constitutive_tools stress_tools solver_tools)

# Add project source directories
add_subdirectory ("src/cpp")
if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    add_subdirectory ("src/cpp/tests")
    add_subdirectory ("docs")
endif()

# Set the c++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)