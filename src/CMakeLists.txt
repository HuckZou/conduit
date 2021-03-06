###############################################################################
# Copyright (c) 2014-2017, Lawrence Livermore National Security, LLC.
#
# Produced at the Lawrence Livermore National Laboratory
#
# LLNL-CODE-666778
#
# All rights reserved.
#
# This file is part of Conduit.
#
# For details, see: http://software.llnl.gov/conduit/.
#
# Please also read conduit/LICENSE
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#
# * Redistributions of source code must retain the above copyright notice,
#   this list of conditions and the disclaimer below.
#
# * Redistributions in binary form must reproduce the above copyright notice,
#   this list of conditions and the disclaimer (as noted below) in the
#   documentation and/or other materials provided with the distribution.
#
# * Neither the name of the LLNS/LLNL nor the names of its contributors may
#   be used to endorse or promote products derived from this software without
#   specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL LAWRENCE LIVERMORE NATIONAL SECURITY,
# LLC, THE U.S. DEPARTMENT OF ENERGY OR CONTRIBUTORS BE LIABLE FOR ANY
# DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
# DAMAGES  (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
# OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
# HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT,
# STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING
# IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
###############################################################################

cmake_minimum_required(VERSION 3.0)
################################
# cmake policy selections
################################
# avoid default mac osx rpath settings for cmake 3.0
cmake_policy(SET CMP0042 OLD)
# cmake var deref policy
if(POLICY CMP0054)
  cmake_policy(SET CMP0054 OLD)
endif()


################################
# Conduit
################################

project(conduit VERSION "0.2.1")

################################
# Build Options
################################
option(BUILD_SHARED_LIBS  "Build shared libraries"      ON)
option(ENABLE_TESTS       "Build conduit tests"         ON)
option(ENABLE_DOCS        "Build conduit documentation" ON)
option(ENABLE_COVERAGE    "Build with coverage flags"   OFF)

option(ENABLE_PYTHON      "Build Python Support"        OFF)
option(ENABLE_FORTRAN     "Build Fortran  support"      OFF)

option(ENABLE_MPI         "Build MPI Support"           OFF)

################################
# Invoke CMake Fortran setup
# if ENABLE_FORTRAN == ON
################################
if(ENABLE_FORTRAN)
    enable_language(Fortran)
endif()

################################
# Set some standard cmake opts
################################
include(CMake/CMakeBasics.cmake)

################################
# Fortran Support
################################
include(CMake/SetupFortran.cmake)

################################
# Checks for type sizes, etc
################################
include(CMake/BasicTypeChecks.cmake)

################################
# Setup 3rd Party Libs
################################
include(CMake/Setup3rdParty.cmake)

################################
# Setup docs targets
################################
include(CMake/SetupDocs.cmake)

################################
# Setup tests helpers
################################
include(CMake/SetupTests.cmake)

################################
# Setup project wide includes
################################
include(CMake/SetupIncludes.cmake)

################################
# Add our libs
################################
add_subdirectory(libs)

################################
# Add docs
################################
if(ENABLE_DOCS)
    add_subdirectory(docs)
else()
    message(STATUS "Skipping documentation targets (ENABLE_DOCS = OFF)")
endif()

################################
# Add our tests
################################
if(ENABLE_TESTS)
    add_subdirectory(tests)
else()
    message(STATUS "Skipping test targets (ENABLE_TESTS = OFF)")
endif()


################################
# Create CMake importable
# exports for all of our targets
################################
install(EXPORT conduit DESTINATION lib/cmake)
