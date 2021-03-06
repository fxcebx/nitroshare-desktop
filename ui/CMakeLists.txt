configure_file(config.h.in "${CMAKE_CURRENT_BINARY_DIR}/config.h")

set(SRC
    main.cpp
)

# Include the resource file
if(WIN32)
    set(SRC ${SRC} resource.rc)
endif()

add_executable(nitroshare-ui WIN32 ${SRC})

set_target_properties(nitroshare-ui PROPERTIES
    CXX_STANDARD 11
)

target_include_directories(nitroshare-ui PUBLIC
    "${CMAKE_CURRENT_BINARY_DIR}"
)

target_link_libraries(nitroshare-ui nitroshare Qt5::Core Qt5::Widgets)

install(TARGETS nitroshare-ui
    DESTINATION "${INSTALL_BIN_PATH}"
)

if(WIN32)
    # Run windeployqt on the binary
    include(DeployQt)
    windeployqt(nitroshare-ui)

    # If the exe target exists, add the binary as a dependency
    if(INNOSETUP_EXECUTABLE)
        add_dependencies(exe nitroshare-ui)
    endif()
endif()

if(APPLE)
    include(DeployQt)
    macdeployqt(nitroshare-ui)
endif()
