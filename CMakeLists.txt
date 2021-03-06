cmake_policy(SET CMP0015 OLD)

# Set project name
project (TEdit)

# Set minimum version
cmake_minimum_required (VERSION 2.8.6)
if (COMMAND cmake_policy)
    cmake_policy (SET CMP0003 NEW)
    if (CMAKE_VERSION VERSION_GREATER 2.8.12 OR CMAKE_VERSION VERSION_EQUAL 2.8.12)
        cmake_policy (SET CMP0022 NEW) # INTERFACE_LINK_LIBRARIES defines the link interface
    endif ()
    if (CMAKE_VERSION VERSION_GREATER 3.0.0 OR CMAKE_VERSION VERSION_EQUAL 3.0.0)
        cmake_policy (SET CMP0026 OLD) # Disallow use of the LOCATION target property - therefore we set to OLD as we still need it
        cmake_policy (SET CMP0042 NEW) # MACOSX_RPATH is enabled by default
    endif ()
endif ()

# Set CMake modules search path
set (CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/CMake/Modules)
MESSAGE("${CMAKE_MODULE_PATH}")
# Include Urho3D Cmake common module
include (Urho3D-CMake-common)

# Find Urho3D library
find_package (Urho3D REQUIRED)
include_directories(${URHO3D_HOME}/include/Urho3D/ThirdParty/Lua${JIT})
include_directories(${URHO3D_HOME}/include/Urho3D/ThirdParty/toluapp)
include_directories(${URHO3D_HOME}/include/Urho3D/ThirdParty/AngelScript)
include_directories (${URHO3D_INCLUDE_DIRS})
include_directories (${INCLUDE_DIRS_ONLY})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(USETHREAD 1)

add_subdirectory(ThirdParty/ANL)

set(INCLUDE_DIRS_ONLY ${INCLUDE_DIRS_ONLY} ThirdParty/ANL/VM ThirdParty/ANL/Imaging )
set(LIBS ${LIBS} ANL) 

# Define target name
set (TARGET_NAME TEdit)


# Define source files
define_source_files (EXTRA_CPP_FILES ThirdParty/ANL/Bindings/bind_anl.cpp)
# Setup target with resource copying
setup_main_executable ()
