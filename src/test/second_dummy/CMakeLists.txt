# Add all the source files to the add_executable command
# Replace dummy_test with the name of the executable to be generated
# (it will be both the name of the executable and the name of
# the target, so it must be unique within the whole library)
add_executable(second_dummy_test main.cpp)
target_link_libraries(second_dummy_test ${LIBRARIES})

# This allows the test to be executed with ctest
add_test(NAME SecondDummyTest
         COMMAND second_dummy_test input_file1 input_file2 output_file) # this is the command that will be executed to run the test

# If the test needs some files to be copied to the build directory
# we can specify them here. These files will be copied to this build
# directory regardless of their source directory
copy_files_to_binary_dir(second_dummy_test_files # target name
    ../../mesh/input_file1
    ../../mesh/input_file2
)

# If the test needs some files to be linked to the build directory
# we can specify them here. The link will be created in this build
# directory regardless of their source directory
link_files_to_binary_dir(second_dummy_test_files_link # target name
    ../../mesh/large_input_file1
    ../../mesh/large_input_file2
)


