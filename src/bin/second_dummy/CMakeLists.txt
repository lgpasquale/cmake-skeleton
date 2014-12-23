# Replace dummy with the name of the executable to be generated
# (it will be both the name of the executable and the name of
# the target, so it must be unique within the whole library)
add_executable(second_dummy main.cpp)
target_link_libraries(second_dummy ${LIBRARIES})

install (TARGETS second_dummy
    DESTINATION ${INSTALL_BIN_DIR})


