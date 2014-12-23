add_subdirectory(${PROJECT_NAME})
add_subdirectory(bin)

option(${PROJECT_NAME}_ENABLE_TESTS "Enable tests" ON)
if(${PROJECT_NAME}_ENABLE_TESTS)
    enable_testing()
    add_subdirectory(test)
endif()

