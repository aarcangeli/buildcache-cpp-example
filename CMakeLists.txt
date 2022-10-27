# Setup build cache
set(CMAKE_C_COMPILER_LAUNCHER "buildcache")
set(CMAKE_CXX_COMPILER_LAUNCHER "buildcache")

# Setup installation directory
set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_SOURCE_DIR}/install")

# Set Up Universal Build on Mac OS X
set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64")

# Create executable
cmake_minimum_required(VERSION 3.23)
project(example_project)
set(CMAKE_CXX_STANDARD 20)

# On visual studio, pack debug symbols in object files to make build cache happy
if (MSVC)
    string(REPLACE "/Zi" "/Z7" CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG}")
    string(REPLACE "/Zi" "/Z7" CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")
    string(REPLACE "/Zi" "/Z7" CMAKE_C_FLAGS_RELWITHDEBINFO "${CMAKE_C_FLAGS_RELWITHDEBINFO}")
    string(REPLACE "/Zi" "/Z7" CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELWITHDEBINFO}")
endif ()

add_executable(example_project main.cpp)

# Install executable
install(TARGETS example_project DESTINATION "${CMAKE_INSTALL_PREFIX}")

# Install pdb file
if (MSVC)
    install(FILES $<TARGET_PDB_FILE:example_project> DESTINATION "${CMAKE_INSTALL_PREFIX}")
endif ()
