cmake_minimum_required(VERSION 3.5)

project(R-2000iB165F.SLDASM)

find_package(ament_cmake REQUIRED)



find_package(roslaunch)

foreach(dir config launch meshes urdf)
	install(DIRECTORY ${dir}/
		DESTINATION share/${PROJECT_NAME}/${dir})
endforeach(dir)

ament_package()
