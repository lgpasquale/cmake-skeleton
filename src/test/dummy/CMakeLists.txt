# Add all the source files to the add_executable command
# Replace dummy_test with the name of the executable to be generated
# (it will be both the name of the executable and the name of
# the target, so it must be unique within the whole library)
add_project_test_executable(dummy_test main.cpp
    TEST_NAME DummyTest
    ARGUMENTS input_file1 input_file2 output_file) # this are the arguments that will be passed to the test executable

# If the test needs some files to be copied to the build directory
# we can specify them here. These files will be copied to this build
# directory regardless of their source directory
#copy_files_to_binary_dir(dummy_test_files # target name
#    ../../data/input_file1
#    ../../data/input_file2
#)

# If the test needs some files to be linked to the build directory
# we can specify them here. The link will be created in this build
# directory regardless of their source directory
#link_files_to_binary_dir(dummy_test_files_link # target name
#    ../../data/large_input_file1
#    ../../data/large_input_file2
#)


