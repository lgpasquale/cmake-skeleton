# This deletes the variables stored in the cache from previous calls to cmake
set(${PROJECT_NAME}_HEADERS CACHE INTERNAL "")
set(${PROJECT_NAME}_SOURCES CACHE INTERNAL "")

add_subdirectory(dummy)
add_subdirectory(second_dummy)

add_project_library(${PROJECT_NAME} ${${PROJECT_NAME}_SOURCES} ${${PROJECT_NAME}_HEADERS})
install_files_with_directory_structure(${PROJECT_INCLUDE_INSTALL_DIR}/${PROJECT_NAME} ${${PROJECT_NAME}_HEADERS})

