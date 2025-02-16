add_library(animal SHARED
    animal.cpp
)
# Add symbol export definitions
target_compile_definitions(animal PRIVATE BUILDING_ANIMAL)
# Specify include directories
target_include_directories(animal PUBLIC 
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# Install the shared library to lib directory
install(TARGETS animal
    LIBRARY DESTINATION lib
    PUBLIC_HEADER DESTINATION include
)

# Install header files
install(FILES 
    ${CMAKE_SOURCE_DIR}/include/animal.h
    DESTINATION include
)

# Generate and install CMake config files so other projects can find this library
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_BINARY_DIR}/AnimalConfigVersion.cmake"
    VERSION 1.0.0
    COMPATIBILITY SameMajorVersion
)

configure_file(
    "${CMAKE_SOURCE_DIR}/cmake/AnimalConfig.cmake.in"
    "${CMAKE_BINARY_DIR}/AnimalConfig.cmake"
    @ONLY
)

install(FILES
    "${CMAKE_BINARY_DIR}/AnimalConfig.cmake"
    "${CMAKE_BINARY_DIR}/AnimalConfigVersion.cmake"
    DESTINATION lib/cmake/Animal
)


add_subdirectory(lion)



add_executable(main_pipeline 
    main_pipeline.cpp
)
target_link_libraries(main_pipeline PRIVATE
    yaml-cpp
    animal
    ${CMAKE_DL_LIBS}  # Add dlopen support
)