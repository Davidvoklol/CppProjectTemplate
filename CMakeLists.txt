cmake_minimum_required(VERSION 3.10)
project(MyLib C CXX)
string(TOUPPER ${PROJECT_NAME} PROJECT_NAME_UPPER)

set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Library
option(${PROJECT_NAME_UPPER}_BUILD_SHARED_LIBS "Build libraries as shared" OFF)

file(GLOB ${PROJECT_NAME_UPPER}_SOURCE_FILES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*/*.cpp"
)

if(${PROJECT_NAME_UPPER}_BUILD_SHARED_LIBS)
    add_library(${PROJECT_NAME} SHARED ${${PROJECT_NAME_UPPER}_SOURCE_FILES})
else()
    add_library(${PROJECT_NAME} STATIC ${${PROJECT_NAME_UPPER}_SOURCE_FILES})
endif()

target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/include"
)

set_target_properties(${PROJECT_NAME} PROPERTIES 
    PREFIX ""
    IMPORT_PREFIX ""
    OUTPUT_NAME "${PROJECT_NAME}"
)

# Single-config output directories
set_target_properties(${PROJECT_NAME} PROPERTIES 
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/lib"
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/lib"
)

# Multi-config output directories
if(CMAKE_CONFIGURATION_TYPES) 
    foreach(CONFIG ${CMAKE_CONFIGURATION_TYPES}) 
        string(TOUPPER ${CONFIG} CONFIG_UPPER) 
        set_target_properties(${PROJECT_NAME} PROPERTIES
            RUNTIME_OUTPUT_DIRECTORY_${CONFIG_UPPER} "${CMAKE_CURRENT_BINARY_DIR}/${CONFIG}/lib" 
            LIBRARY_OUTPUT_DIRECTORY_${CONFIG_UPPER} "${CMAKE_CURRENT_BINARY_DIR}/${CONFIG}/lib" 
            ARCHIVE_OUTPUT_DIRECTORY_${CONFIG_UPPER} "${CMAKE_CURRENT_BINARY_DIR}/${CONFIG}/lib"
        ) 
    endforeach()
endif()

# Examples
option(${PROJECT_NAME_UPPER}_BUILD_EXAMPLES "Build examples" OFF)
if(${PROJECT_NAME_UPPER}_BUILD_EXAMPLES)
    file(GLOB ${PROJECT_NAME_UPPER}_EXAMPLE_SOURCE_FILES
        "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp"
        "${CMAKE_CURRENT_SOURCE_DIR}/examples/*/*.cpp"
    )

    foreach(example ${${PROJECT_NAME_UPPER}_EXAMPLE_SOURCE_FILES})
        get_filename_component(example_name ${example} NAME_WE)
        add_executable(${example_name} ${example})
        
        # Single-config output directories
        set_target_properties(${example_name} PROPERTIES
            RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/bin/${example_name}"
        )

        # Multi-config output directories
        if(CMAKE_CONFIGURATION_TYPES) 
            foreach(CONFIG ${CMAKE_CONFIGURATION_TYPES}) 
                string(TOUPPER ${CONFIG} CONFIG_UPPER) 
                set_target_properties(${example_name} PROPERTIES 
                    RUNTIME_OUTPUT_DIRECTORY_${CONFIG_UPPER} "${CMAKE_CURRENT_BINARY_DIR}/${CONFIG}/bin/${example_name}"
                )
            endforeach()
        endif()
        
        target_include_directories(${PROJECT_NAME} PRIVATE
            "${CMAKE_CURRENT_SOURCE_DIR}/include"
        )

        target_link_libraries(${example_name} PRIVATE ${PROJECT_NAME})
    endforeach()
endif()