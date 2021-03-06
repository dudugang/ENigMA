
project(ENigMApy)

find_package(PythonLibs 3.0 REQUIRED)
find_package(PythonInterp 3.0 REQUIRED)

include_directories(
  ${EIGEN_DIR}
  ${EIGEN_DIR}/Eigen
  ${VIENNACL_DIR}
  ${RTREE_DIR}
  ${EXPRTK_DIR}
  ../../../src/common
  ../../../src/analytical
  ../../../src/bem
  ../../../src/csg
  ../../../src/geometry
  ../../../src/integration
  ../../../src/fdm
  ../../../src/fem
  ../../../src/fvm
  ../../../src/fvm/flow
  ../../../src/fvm/thermal
  ../../../src/sph
  ../../../src/lbm
  ../../../src/material
  ../../../src/mesh
  ../../../src/pde
  ../../../src/solvers
  ../../../src/post
  ../../../src/sle
  ../../../src/stl
  ${CMAKE_CURRENT_BINARY_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${PYTHON_INCLUDE_PATH}
)

set(ENGIMA_INTERFACE
  ../ENigMA.i
)

#set(CMAKE_SWIG_FLAGS "")

set_source_files_properties(${ENGIMA_INTERFACE} PROPERTIES CPLUSPLUS ON)
set_source_files_properties(${ENGIMA_INTERFACE} PROPERTIES SWIG_FLAGS "-debug-tmused")
set_source_files_properties(${ENGIMA_INTERFACE} PROPERTIES SWIG_MODULE_NAME ENigMA)

swig_add_library(ENigMApy LANGUAGE python SOURCES ${ENGIMA_INTERFACE})
swig_link_libraries(ENigMApy ${PYTHON_LIBRARIES})

if(WIN32)
  add_definitions(/bigobj)
endif(WIN32)

set_target_properties(_ENigMApy PROPERTIES OUTPUT_NAME ENigMA)
add_custom_command(TARGET _ENigMApy POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy ENigMA.py $<TARGET_FILE_DIR:_ENigMApy>/ENigMA.py)

