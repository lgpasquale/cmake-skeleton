# This deletes the variables stored in the cache from previous calls to cmake
set(${PROJECT_NAME}_HEADERS CACHE INTERNAL "")
set(${PROJECT_NAME}_SOURCES CACHE INTERNAL "")

add_subdirectory(dummy)
add_subdirectory(second_dummy)

add_library(${PROJECT_NAME} ${${PROJECT_NAME}_SOURCES} ${${PROJECT_NAME}_HEADERS})
set(${PROJECT_NAME}_LIBRARIES ${${PROJECT_NAME}_LIBRARIES} ${PROJECT_NAME} CACHE INTERNAL "")

target_link_libraries(${PROJECT_NAME} ${LIBRARIES})
install (TARGETS ${${PROJECT_NAME}_LIBRARIES} DESTINATION ${PROJECT_LIB_INSTALL_DIR})
install_files_with_directory_structure(${PROJECT_INCLUDE_INSTALL_DIR}/${PROJECT_NAME} ${${PROJECT_NAME}_HEADERS})

set(LIBRARIES ${${PROJECT_NAME}_LIBRARIES} ${LIBRARIES} CACHE INTERNAL "")

