cmake_minimum_required(VERSION 2.8.3)
project(boost_dll)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(Boost COMPONENTS system thread filesystem)

include_directories(
    ./
  ${Boost_INCLUDE_DIRS}
)

add_library(boost_dll_plugin SHARED
  boost_dll_plugin.cpp
)
target_link_libraries(boost_dll_plugin ${Boost_LIBRARIES})

add_executable(plugin_loader
  plugin_loader.cpp
)
target_link_libraries(boost_dll_plugin ${Boost_LIBRARIES} dl)
target_link_libraries(plugin_loader ${Boost_LIBRARIES} dl)

install(TARGETS boost_dll_plugin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib)
install(TARGETS plugin_loader
  ARCHIVE DESTINATION bin
  LIBRARY DESTINATION bin
  RUNTIME DESTINATION bin)

install(DIRECTORY include/
  DESTINATION include)

