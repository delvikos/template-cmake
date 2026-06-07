cmake_minimum_required (VERSION 4.2)

project (
  "template-cmake"
  VERSION 0.0.0
  #DESCRIPTION "template-cmake description"
  #HOMEPAGE_URL "https://no-url.com"
  LANGUAGES CXX
  )

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

# Dependencies
# find_package()
# FetchContent()
# add_subdirectory("external")
#
# target_link_libraries(${PROJECT_NAME} PRIVATE my_lib_target_name)
# _Dependencies

# Target
file(GLOB_RECURSE ALL_SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")

set(CORE_SRC_FILES ${ALL_SRC_FILES})
list(REMOVE_ITEM CORE_SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp")

add_library(${PROJECT_NAME}_core STATIC ${CORE_SRC_FILES})
target_include_directories(${PROJECT_NAME}_core PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")

add_executable(${PROJECT_NAME} "${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp")
target_link_libraries(${PROJECT_NAME} PRIVATE ${PROJECT_NAME}_core)
# _Target

# Resource
if(IS_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/resource")
    add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        "${CMAKE_CURRENT_SOURCE_DIR}/resource"
        "$<TARGET_FILE_DIR:${PROJECT_NAME}>"
        COMMENT "Copying resources to build directory..."
    )
endif()
# _Resource

# Testing
enable_testing()
if(NOT CMAKE_BUILD_TYPE STREQUAL "Release")
    file(GLOB_RECURSE TEST_FILES "${CMAKE_CURRENT_SOURCE_DIR}/tests/*.cpp")
    
    if(TEST_FILES)
        #
        #
        
        add_executable(${PROJECT_NAME}_tests ${TEST_FILES})
	    target_link_libraries(${PROJECT_NAME}_tests PRIVATE ${PROJECT_NAME}_core)
        
        # Also link extern sources if tests need them.
        # target_link_libraries(ProjectTests PRIVATE ...)
        
        add_test(NAME AllTests COMMAND ProjectTests)
    endif()
endif()
# _Testing