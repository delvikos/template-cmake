# Copy all files from this directory
#add_custom_target(resources_target ALL
#    COMMAND ${CMAKE_COMMAND} -E copy_directory
#    "${CMAKE_CURRENT_SOURCE_DIR}"
#    "$<TARGET_FILE_DIR:${TARGET_NAME}>"
#    COMMENT "Copying resources to build directory..."
#)
###

#######

# Manual list for copying
set(RESOURCES_TO_COPY
    # FILE_NAME
    # FILE_NAME
)

add_custom_target(copy_resources_target ALL)
foreach(FILE_NAME ${RESOURCES_TO_COPY})
    add_custom_command(
        TARGET copy_resources_target POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            "${CMAKE_CURRENT_SOURCE_DIR}/${FILE_NAME}"
            "$<TARGET_FILE_DIR:${TARGET_NAME}>/${FILE_NAME}"
    )
endforeach()
###