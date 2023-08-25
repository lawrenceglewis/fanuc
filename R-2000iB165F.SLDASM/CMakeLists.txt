cmake_minimum_required(VERSION 2.8.3)

project(R-2000iB165F.SLDASM)

find_package(ament_cmake REQUIRED)

catkin_package()

find_package(roslaunch)

foreach(dir config launch meshes urdf)
	install(DIRECTORY ${dir}/
		DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach(dir)
