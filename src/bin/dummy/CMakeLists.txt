# Replace dummy with the name of the executable to be generated
# (it will be both the name of the executable and the name of
# the target, so it must be unique within the whole library)
add_executable(dummy main.cpp)
target_link_libraries(dummy ${LIBRARIES})

install (TARGETS dummy
    DESTINATION ${PROJECT_BIN_INSTALL_DIR})


