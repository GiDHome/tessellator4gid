message(STATUS "Creating build system for tessellator-app")

add_library(tessellator-app
    "vtkIO.cpp"
    "launcher.cpp"
)

find_package(VTK COMPONENTS 
  vtkCommonCore
  vtkIOGeometry
  vtkIOLegacy
  vtkFiltersCore
)
include_directories(${VTK_INCLUDE_DIRS})

find_package(Boost COMPONENTS program_options serialization)
include_directories(${Boost_INCLUDE_DIR})

find_package(nlohmann_json)

if ( CMAKE_BUILD_TYPE STREQUAL "Debug")
    link_directories(${Boost_LIBRARY_DIR_DEBUG})
else ( CMAKE_BUILD_TYPE STREQUAL "Debug")
    link_directories(${Boost_LIBRARY_DIR_RELEASE})
endif ( CMAKE_BUILD_TYPE STREQUAL "Debug")

if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    set(ADD_BOOS_LIBS "Boost::program_options")
else (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    # on windows boost libs are linked twice
    set(ADD_BOOS_LIBS "")
endif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")

target_link_libraries(tessellator-app
    ${VTK_LIBRARIES}
    ${ADD_BOOS_LIBS}
    nlohmann_json::nlohmann_json
)
#Boost::program_options

add_executable(tessellator
    "tessellator.cpp"
)

target_link_libraries(tessellator tessellator-app tessellator-meshers)
