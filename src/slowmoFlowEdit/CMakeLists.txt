
include_directories(${slowmoVideo_SOURCE_DIR})

set(SRCS
    main.cpp
    mainwindow.cpp
    flowEditCanvas.cpp
    shortcutListDialog.cpp
)

if(APPLE)
        set(BUNDLE "slowmoFlowEdit")
        set(ICONS_DIR "${${PROJECT_NAME}_SOURCE_DIR}/slowmoVideo/slowmoUI/res")
        message( "OS X build" )
        set(MACOSX_BUNDLE_INFO_STRING "${BUNDLE} ${PROJECT_VERSION}")
        set(MACOSX_BUNDLE_BUNDLE_VERSION "${BUNDLE} ${PROJECT_VERSION}")
        set(MACOSX_BUNDLE_LONG_VERSION_STRING "${BUNDLE} ${PROJECT_VERSION}")
        set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${PROJECT_VERSION}")
        set(MACOSX_BUNDLE_COPYRIGHT "${PROJECT_COPYRIGHT_YEAR} ${PROJECT_VENDOR}")
        set(MACOSX_BUNDLE_ICON_FILE "slowmoUI.icns")
        set(MACOSX_BUNDLE_GUI_IDENTIFIER "${PROJECT_DOMAIN_SECOND}.${PROJECT_DOMAIN_FIRST}")
        set(MACOSX_BUNDLE_BUNDLE_NAME "${BUNDLE}")

        set(MACOSX_BUNDLE_RESOURCES "${CMAKE_CURRENT_BINARY_DIR}/${BUNDLE}.app/Contents/Resources")
        set(MACOSX_BUNDLE_ICON "${ICONS_DIR}/${MACOSX_BUNDLE_ICON_FILE}")
        SET_SOURCE_FILES_PROPERTIES(
           ${MACOSX_BUNDLE_ICON}
            PROPERTIES MACOSX_PACKAGE_LOCATION Resources)
        message(STATUS "Bundle will be : ${MACOSX_BUNDLE} =>  ${PROJECT_NAME} ")

        set( SRCS ${SRCS} ${MACOSX_BUNDLE_ICON} )

endif()

include_directories(..)
include_directories(${CMAKE_BINARY_DIR}/slowmoVideo/slowmoFlowEdit)
include_directories(${CMAKE_BINARY_DIR}/slowmoVideo/libgui)

add_executable(slowmoFlowEdit WIN32 MACOSX_BUNDLE ${SRCS})
qt5_use_modules(slowmoFlowEdit Widgets Gui  Core )
target_link_libraries(slowmoFlowEdit sVgui sVflow sVvis sVflowtools)

#install(TARGETS slowmoFlowEdit
#        BUNDLE DESTINATION . COMPONENT Runtime
#        RUNTIME DESTINATION ${DEST} COMPONENT Runtime)

        #install(TARGETS slowmoUI DESTINATION ".")
        #install(TARGETS slowmoFlowEdit DESTINATION ${DEST})

include(DeployQt5) # 2.8.7 or later


if (APPLE)
       install(TARGETS slowmoFlowEdit DESTINATION ".")
       install_qt5_executable(slowmoFlowEdit.app "" ""  )
elseif(WIN32)
       install(TARGETS slowmoFlowEdit DESTINATION ".")
       install_qt5_executable(slowmoFlowEdit.exe "" ""  )
else()
       install(TARGETS slowmoFlowEdit DESTINATION ${DEST})
#       install_qt5_executable(slowmoFlowEdit "" ""  )
endif()

