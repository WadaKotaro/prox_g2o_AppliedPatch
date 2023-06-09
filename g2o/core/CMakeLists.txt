add_library(core ${G2O_LIB_TYPE}
dynamic_aligned_buffer.hpp
ownership.h
base_edge.h
base_multi_edge.h
base_fixed_sized_edge.hpp    base_fixed_sized_edge.h
hyper_graph_action.cpp       hyper_graph_action.h
base_variable_sized_edge.hpp base_variable_sized_edge.h
hyper_graph.cpp              hyper_graph.h
base_unary_edge.h
base_binary_edge.h
base_ternary_edge.h
linear_solver.h
marginal_covariance_cholesky.cpp marginal_covariance_cholesky.h
base_vertex.hpp                  base_vertex.h
matrix_structure.cpp             matrix_structure.h
batch_stats.cpp                  batch_stats.h
openmp_mutex.h
block_solver.hpp      block_solver.h
parameter.cpp         parameter.h
cache.cpp             cache.h
optimizable_graph.cpp optimizable_graph.h
solver.cpp            solver.h
creators.h
optimization_algorithm_factory.cpp optimization_algorithm_factory.h
estimate_propagator.cpp            estimate_propagator.h
factory.cpp                        factory.h
optimization_algorithm_property.h
sparse_block_matrix.hpp                 sparse_block_matrix.h
sparse_optimizer.cpp                    sparse_optimizer.h
hyper_dijkstra.cpp                      hyper_dijkstra.h
parameter_container.cpp                 parameter_container.h
optimization_algorithm.cpp              optimization_algorithm.h
optimization_algorithm_with_hessian.cpp optimization_algorithm_with_hessian.h
optimization_algorithm_gauss_newton.cpp optimization_algorithm_gauss_newton.h
optimization_algorithm_levenberg.cpp    optimization_algorithm_levenberg.h
optimization_algorithm_dogleg.cpp       optimization_algorithm_dogleg.h
sparse_optimizer_terminate_action.cpp   sparse_optimizer_terminate_action.h
jacobian_workspace.cpp                  jacobian_workspace.h
robust_kernel.cpp                       robust_kernel.h
robust_kernel_impl.cpp                  robust_kernel_impl.h
robust_kernel_factory.cpp               robust_kernel_factory.h
io_helper.h
g2o_core_api.h
)

target_include_directories(core PUBLIC
  "$<BUILD_INTERFACE:${g2o_SOURCE_DIR};${PROJECT_BINARY_DIR}>"
  $<INSTALL_INTERFACE:include/g2o/core>
)

set_target_properties(core PROPERTIES OUTPUT_NAME ${LIB_PREFIX}core)
target_link_libraries(core PUBLIC stuff ${G2O_EIGEN3_EIGEN_TARGET})
if(G2O_USE_VENDORED_CERES)
  target_compile_definitions(core PUBLIC "G2O_USE_VENDORED_CERES")
  target_link_libraries(core PUBLIC g2o_ceres_ad)
else()
  find_package(Ceres REQUIRED)
  target_link_libraries(core PUBLIC ceres)
endif()
target_compile_features(core PUBLIC cxx_std_14)

if (APPLE)
  set_target_properties(core PROPERTIES INSTALL_NAME_DIR "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}")
endif()

install(TARGETS core
  EXPORT ${G2O_TARGETS_EXPORT_NAME}
  RUNTIME DESTINATION ${RUNTIME_DESTINATION}
  LIBRARY DESTINATION ${LIBRARY_DESTINATION}
  ARCHIVE DESTINATION ${ARCHIVE_DESTINATION}
  INCLUDES DESTINATION ${INCLUDES_DESTINATION}
)

file(GLOB headers "${CMAKE_CURRENT_SOURCE_DIR}/*.h" "${CMAKE_CURRENT_SOURCE_DIR}/*.hpp")

install(FILES ${headers} DESTINATION ${INCLUDES_INSTALL_DIR}/core)
