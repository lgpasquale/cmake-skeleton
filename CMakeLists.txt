cmake_minimum_required(VERSION 2.8)

set(PROJECT_NAME Awesome)
project(${PROJECT_NAME})

set (${PROJECT_NAME}_MAJOR_VERSION 0)
set (${PROJECT_NAME}_MINOR_VERSION 1)
set (${PROJECT_NAME}_PATCH_VERSION 0)
set(${PROJECT_NAME}_VERSION
    ${${PROJECT_NAME}_MAJOR_VERSION}.${${PROJECT_NAME}_MINOR_VERSION}.${${PROJECT_NAME}_PATCH_VERSION})
# This is used when configuring files
# (this way we have a variable which is not dependent on the project name)
set(PROJECT_VERSION ${${PROJECT_NAME}_VERSION})

#================================
# Third Party Libraries
#================================
# find_package uses a Find<package>.cmake module which it tries to find
# in CMAKE_MODULE_PATH. Since for some packages (umfpack, cholmod, qhull...)
# there is no default module, we use those in ${CMAKE_CURRENT_SOURCE_DIR}/cmake
# (we later restore CMAKE_MODULE_PATH its value).
# CMAKE_PREFIX_PATH is where find_package, find_library and find_file
# look for files. We set this to the user defined TPL_<package>_DIR
# so that system installations of the library don't prevail
set(ENV_CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH})
set(ENV_CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH})
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
# CHOLMOD
set(TPL_SuiteSparse_DIR "" CACHE PATH "SuiteSparse installation base directory")
set(CMAKE_PREFIX_PATH ${TPL_SuiteSparse_DIR})
find_package(Cholmod QUIET)
if(NOT CHOLMOD_FOUND)
    message(FATAL_ERROR "Cholmod not found. Try setting TPL_SuiteSparse_DIR to the installation path of SuiteSparse")
endif()
# UMFPACK
set(CMAKE_PREFIX_PATH ${TPL_SuiteSparse_DIR})
find_package(Umfpack QUIET)
if(NOT UMFPACK_FOUND)
    message(FATAL_ERROR "Umfpack not found. Try setting TPL_SuiteSparse_DIR to the installation path of SuiteSparse")
endif()
# Eigen
set(TPL_Eigen_DIR "" CACHE PATH "Eigen installation base directory")
set(CMAKE_PREFIX_PATH ${TPL_Eigen_DIR})
find_package(Eigen3 QUIET)
if(NOT EIGEN3_FOUND)
    message(FATAL_ERROR "Eigen not found. Try setting TPL_Eigen_DIR to the installation path of Eigen")
endif()
#Qhull
set(CMAKE_PREFIX_PATH ${TPL_Qhull_DIR})
FIND_PACKAGE(Qhull)
if(NOT QHULL_FOUND)
    message(FATAL_ERROR "Qhull not found. Try setting TPL_Qhull_DIR to the installation path of Qhull")
endif()
# Restore CMAKE_MODULE_PATH to its original value
set(CMAKE_MODULE_PATH ${ENV_CMAKE_MODULE_PATH})
# BLAS
set(TPL_BLAS_DIR "" CACHE PATH "BLAS installation base directory")
set(CMAKE_PREFIX_PATH ${TPL_BLAS_DIR})
find_package(BLAS QUIET)
if(NOT BLAS_FOUND)
    message(FATAL_ERROR "BLAS not found. Try setting TPL_BLAS_DIR to the installation path of BLAS")
endif()
# Restore CMAKE_PREFIX_PATH to its original value
set(CMAKE_PREFIX_PATH ${ENV_CMAKE_PREFIX_PATH})
# Boost
set(TPL_Boost_DIR "" CACHE PATH "Boost installation base directory")
set(CMAKE_PREFIX_PATH ${TPL_Boost_DIR})
set(BOOST_ROOT ${TPL_Boost_DIR})
FIND_PACKAGE(Boost REQUIRED COMPONENTS system filesystem regex)
if(NOT BLAS_FOUND)
    message(FATAL_ERROR "Boost not found. Try setting TPL_Boost_DIR to the installation path of Boost")
endif()
# HDF5
set(TPL_HDF5_DIR "" CACHE PATH "HDF5 installation base directory")
set(CMAKE_PREFIX_PATH ${TPL_HDF5_DIR})
set(HDF5_ROOT ${TPL_HDF5_DIR})
FIND_PACKAGE(HDF5 REQUIRED COMPONENTS C CXX)
if(NOT HDF5_FOUND)
    message(FATAL_ERROR "HDF5 not found. Try setting TPL_HDF5_DIR to the installation path of HDF5")
endif()
# zlib
set(TPL_ZLIB_DIR "" CACHE PATH "ZLIB installation base directory")
set(CMAKE_PREFIX_PATH ${TPL_ZLIB_DIR})
if("${TPL_ZLIB_DIR}" STREQUAL "")
FIND_PACKAGE(ZLIB)
else("${TPL_ZLIB_DIR}" STREQUAL "")
set(ZLIB_ROOT ${TPL_ZLIB_DIR})
FIND_PACKAGE(ZLIB)
endif("${TPL_ZLIB_DIR}" STREQUAL "")
if(NOT ZLIB_FOUND)
    message(FATAL_ERROR "ZLIB not found. Try setting TPL_ZLIB_DIR to the installation path of ZLIB")
endif()
# VTK
set(TPL_VTK_DIR "" CACHE PATH "VTK installation base directory")
set(CMAKE_PREFIX_PATH ${TPL_VTK_DIR})
set(VTK_DIR ${TPL_VTK_DIR})
FIND_PACKAGE(VTK REQUIRED)
if(NOT VTK_FOUND)
    message(FATAL_ERROR "VTK not found. Try setting TPL_VTK_DIR to the installation path of VTK")
endif()
INCLUDE(${VTK_USE_FILE})
# Trilinos
set(TPL_Trilinos_DIR "" CACHE PATH "Trilinos installation base directory")
set(CMAKE_PREFIX_PATH ${TPL_TRILINOS_DIR})
set(Trilinos_DIR ${TPL_Trilinos_DIR}/lib/cmake/Trilinos/)
FIND_PACKAGE(Trilinos REQUIRED)
if(NOT Trilinos_FOUND)
    message(FATAL_ERROR "Trilinos not found. Try setting TPL_Trilinos_DIR to the installation path of Trilinos")
endif()
# Restore CMAKE_PREFIX_PATH to its original value
set(CMAKE_PREFIX_PATH ${ENV_CMAKE_PREFIX_PATH})

# This file contains the definition of special functions
include(${CMAKE_CURRENT_SOURCE_DIR}/cmake/IncludeFunctions.cmake)

set(${PROJECT_NAME}_TPL_INCLUDE_DIRS
    ${CHOLMOD_INCLUDES}
    ${UMFPACK_INCLUDES}
    ${EIGEN3_INCLUDE_DIR}
    ${Boost_INCLUDE_DIRS}
    ${HDF5_INCLUDE_DIR}
    ${HDF5_INCLUDE_DIR_CPP}
    ${Trilinos_INCLUDE_DIRS}
    ${Trilinos_TPL_INCLUDE_DIRS}
    ${VTK_INCLUDE_DIRS}
    )
message(STATUS "${PROJECT_NAME}_TPL_INCLUDE_DIRS = ${${PROJECT_NAME}_TPL_INCLUDE_DIRS}")

set(${PROJECT_NAME}_TPL_LIBRARY_DIRS
    ${Boost_LIBRARY_DIRS}
    ${HDF5_LIBRARY_DIRS}
    ${Trilinos_LIBRARY_DIRS}
    ${VTK_LIBRARY_DIRS}
    )
message(STATUS "${PROJECT_NAME}_TPL_LIBRARY_DIRS = ${${PROJECT_NAME}_TPL_LIBRARY_DIRS}")

include_directories(${${PROJECT_NAME}_TPL_INCLUDE_DIRS})

include_directories(".")
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)

link_directories(${${PROJECT_NAME}_TPL_LIBRARY_DIRS})

set(LIBRARIES
    ${UMFPACK_LIBRARIES}
    ${CHOLMOD_LIBRARIES}
    ${BLAS_LIBRARIES}
    ${Boost_LIBRARIES}
    ${HDF5_LIBRARIES}
    ${Trilinos_LIBRARIES}
    ${Trilinos_TPL_LIBRARIES}
    ${VTK_LIBRARIES}
    ${ZLIB_LIBRARIES}
    gfortran CACHE INTERNAL "")
message(STATUS "LIBRARIES = ${LIBRARIES}")

#================================
# Flags
#================================
SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -O2")
IF(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    IF(CMAKE_CXX_COMPILER_VERSION VERSION_GREATER 4.7 OR CMAKE_CXX_COMPILER_VERSION VERSION_EQUAL 4.7)
        SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-unused-local-typedefs")
    ENDIF()
ENDIF()

# Set specific flags for the three different build types (Release, debug and Profile)
# -Wno-unused-local-typedefs suppresses a lot of warnings generated by the debug version of boost
set(CMAKE_CXX_FLAGS_RELEASE "-DNDEBUG")
set(CMAKE_CXX_FLAGS_DEBUG "-D_GLIBCXX_DEBUG -DDEBUG -g -O0")
set(CMAKE_CXX_FLAGS_PROFILE "-pg")
set(CMAKE_CXX_FLAGS_CODECOVERAGE "-g -O0 --coverage")

if("${CMAKE_BUILD_TYPE}" STREQUAL "")
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release Profile CodeCoverage." FORCE)
endif()

IF(CMAKE_BUILD_TYPE STREQUAL "Release")
    message(STATUS "Compiling with flags:\n  ${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS_RELEASE}")
ELSEIF(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message(STATUS "Compiling with flags:\n  ${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS_DEBUG}")
ELSEIF(CMAKE_BUILD_TYPE STREQUAL "Profile")
    message(STATUS "Compiling with flags:\n  ${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS_PROFILE}")
ELSEIF(CMAKE_BUILD_TYPE STREQUAL "CodeCoverage")
    include(${CMAKE_CURRENT_SOURCE_DIR}/cmake/CodeCoverage.cmake)
    message(STATUS "Compiling with flags:\n  ${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS_CODECOVERAGE}")
    setup_target_for_coverage(${PROJECT_NAME}_coverage ${CMAKE_CTEST_COMMAND} coverage)
ENDIF()

#================================
# Installation directories
#================================
# Offer the user the choice of overriding the installation directories
set(INSTALL_LIB_DIR lib CACHE PATH "Installation directory for libraries")
set(INSTALL_BIN_DIR bin CACHE PATH "Installation directory for executables")
set(INSTALL_INCLUDE_DIR include CACHE PATH
  "Installation directory for header files")

if(WIN32 AND NOT CYGWIN)
  set(DEF_INSTALL_CMAKE_DIR CMake)
else()
  set(DEF_INSTALL_CMAKE_DIR lib/cmake/${PROJECT_NAME})
endif()
set(INSTALL_CMAKE_DIR ${DEF_INSTALL_CMAKE_DIR} CACHE PATH
  "Installation directory for CMake files")

mark_as_advanced(INSTALL_LIB_DIR INSTALL_BIN_DIR INSTALL_INCLUDE_DIR INSTALL_CMAKE_DIR)

#================================
# Subdirectories
#================================
# ${PROJECT_NAME}_LIBS refers to the libraries in the "lib" folder
set(${PROJECT_NAME}_LIBS_INCLUDE_DIRS CACHE INTERNAL "")
set(${PROJECT_NAME}_LIBS_LIBRARY_DIRS CACHE INTERNAL "")
set(${PROJECT_NAME}_LIBS_LIBRARIES CACHE INTERNAL "")
add_subdirectory(lib)

include_directories(${${PROJECT_NAME}_LIBS_INCLUDE_DIRS})
link_directories(${${PROJECT_NAME}_LIBS_LIBRARY_DIRS})
set(LIBRARIES ${LIBRARIES} ${${PROJECT_NAME}_LIBS_LIBRARIES} CACHE INTERNAL "")

add_subdirectory(src)

#================================
# Doxygen
#================================

# We add a target named "doc" that generates documentation
# with doxygen
find_package(Doxygen)
find_package(LATEX)
if(DOXYGEN_FOUND)
    set(GENERATE_LATEX_DOC "NO")
    # disabled because bad doxygen documentation is generated by some classes
    # that abuse Qt-style comments (/*! description */) without properly
    # generating a brief description in the first line
#    IF(PDFLATEX_COMPILER)
#        set(GENERATE_LATEX_DOC "YES")
#    ENDIF()
    # generate Doxyfile
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile @ONLY)

    # add doc target
    add_custom_target(doc
        ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating API documentation with Doxygen" VERBATIM
        )
    install(
        DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/doc/html
        DESTINATION share/doc/${PROJECT_NAME}
        COMPONENT doc
        OPTIONAL
        )

    # add pdf-doc target
    # (disabled because bad doxygen documentation is generated by some classes
    # that abuse Qt-style comments (/*! description */) without properly
    # generating a brief description in the first line )
#    IF(PDFLATEX_COMPILER)
#        add_custom_target(pdf-doc
#            ${CMAKE_BUILD_TOOL}
#            DEPENDS doc
#            WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/doc/latex
#            COMMENT "Generating pdf API documentation with latex" VERBATIM
#            )
#        install(
#            FILES ${CMAKE_CURRENT_BINARY_DIR}/doc/latex/refman.pdf
#            DESTINATION share/doc/${PROJECT_NAME}
#            COMPONENT pdf-doc
#            OPTIONAL
#            )
#    ENDIF()


endif(DOXYGEN_FOUND)

#================================
# ${PROJECT_NAME}Config.cmake
#================================

# This file configures and prepares for installation
# ${PROJECT_NAME}Config.cmake and ${PROJECT_NAME}ConfigVersion.cmake
include(${CMAKE_CURRENT_SOURCE_DIR}/cmake/InstallCMakeConfigFiles.cmake)
