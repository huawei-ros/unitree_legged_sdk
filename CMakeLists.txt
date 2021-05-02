cmake_minimum_required(VERSION 3.5)
project(unitree_legged_sdk)

# include_directories(include)

# link_directories(lib)

# add_compile_options(-std=c++11)

# set(EXTRA_LIBS -pthread libunitree_legged_sdk_amd64.so lcm)

# set(CMAKE_CXX_FLAGS "-O3")

# # one pc one process
# add_executable(example_position examples/example_position.cpp)
# target_link_libraries(example_position ${EXTRA_LIBS})

# add_executable(example_velocity examples/example_velocity.cpp)
# target_link_libraries(example_velocity ${EXTRA_LIBS})

# add_executable(example_torque examples/example_torque.cpp)
# target_link_libraries(example_torque ${EXTRA_LIBS})

# add_executable(example_walk examples/example_walk.cpp)
# target_link_libraries(example_walk ${EXTRA_LIBS})

# # one pc multi process
# add_executable(lcm_server_high examples/lcm_server_high.cpp)
# target_link_libraries(lcm_server_high ${EXTRA_LIBS})

# add_executable(lcm_server_low examples/lcm_server_low.cpp)
# target_link_libraries(lcm_server_low ${EXTRA_LIBS})

# # multi pc
# add_executable(udp_send_test examples/multi_pc_udp_send.cpp)
# target_link_libraries(udp_send_test ${EXTRA_LIBS})

# add_executable(udp_recv_test examples/multi_pc_udp_recv.cpp)
# target_link_libraries(udp_recv_test ${EXTRA_LIBS})

# # lcm server
# add_executable(lcm_server examples/lcm_server.cpp)
# target_link_libraries(lcm_server ${EXTRA_LIBS})

find_package(ament_cmake REQUIRED)

add_library(${PROJECT_NAME} SHARED IMPORTED)

if(CMAKE_SYSTEM_PROCESSOR STREQUAL aarch64)
    set_property(TARGET ${PROJECT_NAME} PROPERTY IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/lib/libunitree_legged_sdk_arm64.so)
    INSTALL(
        FILES lib/libunitree_legged_sdk_arm64.so
        DESTINATION lib
    )
elseif(CMAKE_SYSTEM_PROCESSOR STREQUAL x86_64)
    set_property(TARGET ${PROJECT_NAME} PROPERTY IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/lib/libunitree_legged_sdk_amd64.so)
    INSTALL(
        FILES lib/libunitree_legged_sdk_amd64.so
        DESTINATION lib
    )
else()
    message(FATAL_ERROR "${PROJECT_NAME} doesn't support architecture ${CMAKE_SYSTEM_PROCESSOR}")
endif()

install(DIRECTORY include/
  DESTINATION include/
)

ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})
ament_package()