cmake_minimum_required(VERSION 3.14)

project(lspcpp-boilerplate)

# set(CMAKE_CXX_STANDARD 11)
# set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(Boost_NO_SYSTEM_PATHS TRUE)
set(Boost_USE_STATIC_LIBS TRUE)

find_package(Boost REQUIRED COMPONENTS date_time chrono filesystem system thread program_options)

add_library(lspcpp STATIC IMPORTED)

# set_property(
#   TARGET lspcpp APPEND PROPERTY IMPORTED_CONFIGURATIONS DEBUG
# )

# set_target_properties(lspcpp PROPERTIES
#   IMPORTED_LINK_INTERFACE_LANGUAGES_DEBUG "CXX"
#   IMPORTED_LOCATION_DEBUG "${CLIP_ROOT}/build/Release/clip.lib"
# )

set_property(   
  TARGET lspcpp APPEND PROPERTY IMPORTED_CONFIGURATIONS RELEASE
)

set_target_properties(lspcpp PROPERTIES
  IMPORTED_LINK_INTERFACE_LANGUAGES_RELEASE "CXX"
  IMPORTED_LOCATION_RELEASE "${LSPCPP_ROOT}/liblspcpp/lib/liblspcpp.a"
)
  
set_target_properties(lspcpp PROPERTIES
  MAP_IMPORTED_CONFIG_MINSIZEREL Release
  MAP_IMPORTED_CONFIG_RELWITHDEBINFO Release
)

target_include_directories(lspcpp INTERFACE 
    "${LSPCPP_ROOT}/liblspcpp/include"
)

add_library(network-uri STATIC IMPORTED)

# set_property(
#   TARGET lspcpp APPEND PROPERTY IMPORTED_CONFIGURATIONS DEBUG
# )

# set_target_properties(lspcpp PROPERTIES
#   IMPORTED_LINK_INTERFACE_LANGUAGES_DEBUG "CXX"
#   IMPORTED_LOCATION_DEBUG "${CLIP_ROOT}/build/Release/clip.lib"
# )

set_property(   
  TARGET network-uri APPEND PROPERTY IMPORTED_CONFIGURATIONS RELEASE
)

set_target_properties(network-uri PROPERTIES
  IMPORTED_LINK_INTERFACE_LANGUAGES_RELEASE "CXX"
  IMPORTED_LOCATION_RELEASE "${LSPCPP_ROOT}/network-uri/lib/libnetwork-uri.a"
)

set_target_properties(network-uri PROPERTIES
  MAP_IMPORTED_CONFIG_MINSIZEREL Release
  MAP_IMPORTED_CONFIG_RELWITHDEBINFO Release
)

target_include_directories(network-uri INTERFACE 
    "${LSPCPP_ROOT}/network-uri/include"
)

SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static")

add_executable(${PROJECT_NAME} 
    "src/main.cpp"
)

target_include_directories(${PROJECT_NAME} PRIVATE
    ${Boost_INCLUDE_DIRS}
)

target_link_libraries(${PROJECT_NAME} WS2_32 lspcpp ${Boost_LIBRARIES} network-uri)