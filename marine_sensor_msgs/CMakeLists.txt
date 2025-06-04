cmake_minimum_required(VERSION 3.5.0)
project(marine_sensor_msgs)

# NOTE This is a hybrid ROS1/ROS2 package. As it contains only messages, this is
# not too arduous

find_package(ros_environment REQUIRED)
set(ROS_VERSION $ENV{ROS_VERSION})

set(MSG_FILES RadarEcho.msg RadarSector.msg TurbidityNTU.msg)

if(${ROS_VERSION} EQUAL 2)

  find_package(ament_cmake REQUIRED)
  find_package(rclpy REQUIRED)
  find_package(rosidl_default_generators REQUIRED)
  find_package(std_msgs REQUIRED)

  # Append "msg/" to each message file
  set(ROS2_MSG_FILES "")
  foreach(msg_file ${MSG_FILES})
    list(APPEND ROS2_MSG_FILES "msg/${msg_file}")
  endforeach()

  rosidl_generate_interfaces(${PROJECT_NAME} ${ROS2_MSG_FILES} DEPENDENCIES
                             std_msgs)

  ament_export_dependencies(rclpy rosidl_default_runtime std_msgs)
  ament_package()

elseif(${ROS_VERSION} EQUAL 1)

  find_package(catkin REQUIRED COMPONENTS message_generation rospy std_msgs)

  add_message_files(FILES ${MSG_FILES})

  generate_messages(DEPENDENCIES std_msgs)

  catkin_package(CATKIN_DEPENDS message_runtime rospy std_msgs)

endif()
