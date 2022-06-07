cmake_minimum_required(VERSION 3.0)

project(mitkCore)

set(CMAKE_EXPORT_PACKAGE_REGISTRY ON)

find_package(itkCommon REQUIRED)
find_package(mbilog REQUIRED)

include(files.cmake)

include (GenerateExportHeader)
include(FeatureSummary)

# Apply properties to the module target.
add_compile_definitions(${PROJECT_NAME}_EXPORTS)

add_library(${PROJECT_NAME} STATIC ${CPP_FILES} ${H_FILES})

#include_directories(${CMAKE_CURRENT_BINARY_DIR})

target_include_directories(${PROJECT_NAME} PUBLIC 
$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
$<INSTALL_INTERFACE:$<INSTALL_PREFIX>/${CMAKE_INSTALL_INCLUDEDIR}>)

target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_BINARY_DIR})

configure_file("${CMAKE_CURRENT_SOURCE_DIR}/mitkCoreConfig.cmake.in"
"${CMAKE_CURRENT_BINARY_DIR}/mitkCoreConfig.cmake" @ONLY)

generate_export_header(${PROJECT_NAME}
  EXPORT_MACRO_NAME MITKCORE_EXPORT
  EXPORT_FILE_NAME MitkCoreExports.h
  )

export(TARGETS ${PROJECT_NAME} FILE "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Targets.cmake")

install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}Targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin
        INCLUDES DESTINATION include
        )

target_link_libraries(${PROJECT_NAME} PUBLIC itkCommon mbilog)

install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
"${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
  DESTINATION lib/cmake/${PROJECT_NAME}
  )