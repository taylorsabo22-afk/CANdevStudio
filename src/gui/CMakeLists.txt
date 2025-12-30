add_subdirectory(projectconfig)

set(srcs
    main.cpp
    mainwindow.cpp
    mainwindow.ui
    toolbar.ui
    aboutdialog.ui
)

add_executable(CANdevStudio MACOSX_BUNDLE ${GUI_TYPE} ${srcs})
target_link_libraries(CANdevStudio projectconfig)
if(ANDROID)
    target_link_libraries(CANdevStudio Qt5::AndroidExtras)
    set_target_properties(CANdevStudio PROPERTIES
        QT_ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_SOURCE_DIR}/android"
        QT_ANDROID_VERSION_NAME "${ANDROID_VERSION_NAME}"
        QT_ANDROID_VERSION_CODE "${ANDROID_VERSION_CODE}"
    )
endif()
target_compile_definitions(CANdevStudio PRIVATE $<$<CONFIG:Debug>:CDS_DEBUG=true> $<$<NOT:$<CONFIG:Debug>>:CDS_DEBUG=false>)
target_include_directories(CANdevStudio 
    PRIVATE
        "${CMAKE_SOURCE_DIR}/components/"
        "${CMAKE_BINARY_DIR}/generated"
)
if(ANDROID)
    install(TARGETS CANdevStudio
        LIBRARY DESTINATION ${INSTALL_DEST_LIB}
        COMPONENT Runtime
    )
else()
    install(TARGETS CANdevStudio 
        BUNDLE DESTINATION . COMPONENT Runtime
        RUNTIME DESTINATION ${INSTALL_DEST_BIN} COMPONENT Runtime
    )
endif()
