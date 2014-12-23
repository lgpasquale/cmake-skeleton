# This deletes the variables stored in the cache from previous calls to cmake
set(${PROJECT_NAME}_HEADERS CACHE INTERNAL "")
set(${PROJECT_NAME}_SOURCES CACHE INTERNAL "")

add_subdirectory(dummy)
add_subdirectory(second_dummy)

add_library(${PROJECT_NAME} ${${PROJECT_NAME}_SOURCES} ${${PROJECT_NAME}_HEADERS})
target_link_libraries(${PROJECT_NAME} ${LIBRARIES} ${${PROJECT_NAME}_LIBS_LIBRARIES})
install (TARGETS ${PROJECT_NAME} DESTINATION ${INSTALL_LIB_DIR})
install_files_with_directory_structure(${INSTALL_INCLUDE_DIR}/${PROJECT_NAME} ${${PROJECT_NAME}_HEADERS})

set(LIBRARIES ${PROJECT_NAME} ${LIBRARIES} CACHE INTERNAL "")

