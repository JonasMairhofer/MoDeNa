cmake_minimum_required (VERSION 2.8)

# I am specifying two projects in the same Cmake file (not sure if good idea)
project (PCSAFT_Density C CXX Fortran)
project (MacroscopicProblem C)

if( CMAKE_VERSION VERSION_GREATER "3.0" )
  cmake_policy(SET CMP0042 OLD)
  cmake_policy(SET CMP0026 OLD)
  cmake_policy(SET CMP0028 OLD)
endif()

set(CMAKE_BUILD_TYPE Release)

add_custom_target(debug
  COMMAND ${CMAKE_COMMAND} -DCMAKE_BUILD_TYPE=Debug ${CMAKE_SOURCE_DIR}
  COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target all
  COMMENT "Switch CMAKE_BUILD_TYPE to Debug"
  )

add_custom_target(release
  COMMAND ${CMAKE_COMMAND} -DCMAKE_BUILD_TYPE=Release ${CMAKE_SOURCE_DIR}
  COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target all
  COMMENT "Switch CMAKE_BUILD_TYPE to Release"
  )

find_package(MODENA REQUIRED)

include(CheckCInline)
check_c_inline(C_INLINE)

# ----------------------------------------------------------------------------
# NOTE:
# This will look through **all** subdirectories and find **any** FORTRAN file
# it will then attempt to to compile and link all the files together.
#                     ------> FRAGILE <------
file(GLOB FORTRANFILES ${CMAKE_CURRENT_SOURCE_DIR}/*/*.f90)
add_executable(PCSAFT_Density ${FORTRANFILES})
set_target_properties(PCSAFT_Density PROPERTIES COMPILE_FLAGS "-fdefault-real-8")
target_link_libraries(PCSAFT_Density MODENA::modena)

# ----------------------------------------------------------------------------
# Compile C project
add_executable(MacroscopicProblem MacroscopicProblem.C)
target_link_libraries(MacroscopicProblem MODENA::modena)


