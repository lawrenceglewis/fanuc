cmake_minimum_required(VERSION 3.0.2)
project(fanuc)
find_package(ament_cmake REQUIRED)
#catkin_metapackage() unfortunatly in ros2 ament doesn't allow you to group packages. What really should be done is replace catkin_metapackage with ament_metapackage.
#Only using one robot right now so i only required the R-2000iB165F.SLDASM if you want the other ones include them.
find_package(fanuc_driver REQUIRED)
find_package(R-2000iB165F.SLDASM REQUIRED)
find_package(fanuc_resources REQUIRED)

ament_target_dependencies(${PROJECT_NAME}
  fanuc_driver 
  R-2000iB165F.SLDASM 
  fanuc_resources)

ament_package()
