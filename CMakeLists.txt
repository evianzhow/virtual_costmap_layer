cmake_minimum_required(VERSION 3.10.2)
project(virtual_costmap_layer)

set(CMAKE_CXX_STANDARD 17)
set(CXX_STANDARD_REQUIRED ON)

# ##############################################################################
# Load and setup packages

set(CATKIN_COMPONENTS
  costmap_2d
  dynamic_reconfigure
  geometry_msgs
  roscpp
  tf
)

find_package(catkin REQUIRED
             ${CATKIN_COMPONENTS}
             message_generation)

# ##############################################################################
# Generate messages and configurations
generate_dynamic_reconfigure_options(cfg/VirtualLayer.cfg)

add_message_files(DIRECTORY msg)

generate_messages(
  DEPENDENCIES
    geometry_msgs
)

# ##############################################################################
# Setup catkin packages
catkin_package(INCLUDE_DIRS
               include
               LIBRARIES
               virtual_layer
               CATKIN_DEPENDS
               ${CATKIN_COMPONENTS}
               message_runtime)

# ##############################################################################
# Generate library
include_directories(include ${catkin_INCLUDE_DIRS})
add_library(virtual_layer src/virtual_layer.cpp)
target_link_libraries(virtual_layer ${catkin_LIBRARIES})
add_dependencies(virtual_layer
    ${PROJECT_NAME}_gencfg
    ${${PROJECT_NAME}_EXPORTED_TARGETS}
    ${catkin_EXPORTED_TARGETS}
    ${PROJECT_NAME}_generate_messages_cpp
)

#############
## Install ##
#############

install(DIRECTORY plugins
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

install(TARGETS virtual_layer
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)

install(DIRECTORY include
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
