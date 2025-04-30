message(STATUS "Creating build system for tessellator-utils")

add_library(tessellator-utils
    "ConvexHull.cpp"
    "CoordGraph.cpp"
    "ElemGraph.cpp"
    "Geometry.cpp"
    "GridTools.cpp"
    "MeshTools.cpp"
    "RedundancyCleaner.cpp"
    "Tools.cpp"
)

find_package(Boost REQUIRED graph)
# include_directories(${Boost_INCLUDE_DIRS})
include_directories(${Boost_INCLUDE_DIR})

if ( CMAKE_BUILD_TYPE STREQUAL "Debug")
    link_directories(${Boost_LIBRARY_DIR_DEBUG})
else ( CMAKE_BUILD_TYPE STREQUAL "Debug")
    link_directories(${Boost_LIBRARY_DIR_RELEASE})
endif ( CMAKE_BUILD_TYPE STREQUAL "Debug")

# target_link_libraries(tessellator-utils Boost::graph)
