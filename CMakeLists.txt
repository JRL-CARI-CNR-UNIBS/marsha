cmake_minimum_required(VERSION 3.0.2)
project(marsha)
add_compile_options(-std=c++17 -funroll-loops -Wall -Ofast)
set(CMAKE_BUILD_TYPE Release)
#set(CMAKE_BUILD_TYPE Debug)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  replanners_lib
  length_penalty_metrics
  ssm15066_estimators
)
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS replanners_lib length_penalty_metrics ssm15066_estimators
  DEPENDS
  )
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  )
add_library(${PROJECT_NAME}
src/MARSHA.cpp
src/replanner_manager_MARSHA.cpp
)
add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

add_executable(crash_test_replanners src/test/crash_test_replanner.cpp)
add_dependencies(crash_test_replanners ${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(crash_test_replanners
${PROJECT_NAME}
${catkin_LIBRARIES}
)

add_executable(crash_test_replanner_managers src/test/crash_test_replanner_manager.cpp)
add_dependencies(crash_test_replanner_managers ${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(crash_test_replanner_managers
${PROJECT_NAME}
${catkin_LIBRARIES}
)
