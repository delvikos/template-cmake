cmake_minimum_required (VERSION 4.2)

project (
  "template-cmake"
  VERSION 0.0.0
  #DESCRIPTION "template-cmake description"
  #HOMEPAGE_URL "https://no-url.com"
  LANGUAGES CXX
  )

option(${PROJECT_NAME}_build_tests "Build project's tests." ON)

# Custom target name can be set here.
set(TARGET_NAME ${PROJECT_NAME})
###

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

# Dependencies
# find_package()
include(FetchContent)
#add_subdirectory("external")
# _Dependencies

# Target
add_executable(${TARGET_NAME} "${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp")
target_include_directories(${TARGET_NAME} PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")

file(GLOB_RECURSE ALL_SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")
set(CORE_SRC_FILES ${ALL_SRC_FILES})
list(REMOVE_ITEM CORE_SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp")

if(CORE_SRC_FILES)
    add_library(${PROJECT_NAME}_core STATIC ${CORE_SRC_FILES})
    target_include_directories(${PROJECT_NAME}_core PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")
    target_link_libraries(${TARGET_NAME} PRIVATE ${PROJECT_NAME}_core)
endif()
# _Target

# Resource
# Uncommit to copy files from resource/ to build directory. Choose files to copy in resource/CMakeLists.txt.
#add_subdirectory("resource")
# _Resource

# Tests
if( (${PROJECT_NAME}_build_tests) AND (NOT CMAKE_BUILD_TYPE STREQUAL "Release") )
    enable_testing()

    # Google Tests template
    FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG        52eb8108c5bdec04579160ae17225d66034bd723 # 1.17.0
    )
    set(BUILD_GMOCK OFF CACHE BOOL "" FORCE)
    FetchContent_MakeAvailable(googletest)
    include(GoogleTest)

    file(GLOB_RECURSE TEST_FILES "${CMAKE_CURRENT_SOURCE_DIR}/tests/*.cpp")
    if(TEST_FILES)
        add_executable(${TARGET_NAME}_tests ${TEST_FILES})
        target_include_directories(${TARGET_NAME}_tests PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")
        target_link_libraries(${TARGET_NAME}_tests PRIVATE gtest_main)

        if(CORE_SRC_FILES)
            target_link_libraries(${TARGET_NAME}_tests PRIVATE ${PROJECT_NAME}_core)
        endif()

        gtest_discover_tests(${TARGET_NAME}_tests)
    endif()
    # _Google Tests template
endif()
# _Tests