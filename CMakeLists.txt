project(qxtglobalshortcut5 CXX)


find_package(Qt5Widgets REQUIRED)
include_directories(${Qt5Gui_PRIVATE_INCLUDE_DIRS})
set(qxtglobalshortcut5_LIBS Qt5::Core Qt5::Gui Qt5::Widgets)

if(UNIX AND NOT APPLE)
  find_package(Qt5X11Extras REQUIRED)
  list(APPEND qxtglobalshortcut5_LIBS Qt5::X11Extras X11)
endif()

add_definitions(-DBUILD_QXT_CORE -DBUILD_QXT_GUI -DQXT_STATIC) # TODO: use properties

set(qxtglobalshortcut5_HEADERS
  "${CMAKE_CURRENT_SOURCE_DIR}/core/qxtglobal.h"
  "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtglobalshortcut_p.h"
  "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtglobalshortcut.h"
  "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtwindowsystem.h")

set(qxtglobalshortcut5_SOURCES
  "${CMAKE_CURRENT_SOURCE_DIR}/core/qxtglobal.cpp"
  "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtwindowsystem.cpp"
  "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtglobalshortcut.cpp")

if(UNIX AND NOT APPLE)
    list(APPEND qxtglobalshortcut5_SOURCES 
        "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtwindowsystem_x11.cpp"
       "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtglobalshortcut_x11.cpp")
    list(APPEND qxtglobalshortcut5_HEADERS 
        "${CMAKE_CURRENT_SOURCE_DIR}/gui/x11info.h")

elseif(APPLE)
    list(APPEND qxtglobalshortcut5_SOURCES 
        "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtwindowsystem_mac.cpp"
        "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtglobalshortcut_mac.cpp")

    list(APPEND qxtglobalshortcut5_HEADERS 
        "${CMAKE_CURRENT_SOURCE_DIR}/qxtwindowsystem_mac.h")

    list(APPEND qxtglobalshortcut5_LIBS 
        "-framework Carbon" "-framework CoreFoundation")
    
elseif(WIN32)
    list(APPEND qxtglobalshortcut5_SOURCES 
        "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtwindowsystem_win.cpp"
        "${CMAKE_CURRENT_SOURCE_DIR}/gui/qxtglobalshortcut_win.cpp")
endif()

add_library(qxtglobalshortcut5 STATIC ${qxtglobalshortcut5_SOURCES})
target_include_directories(qxtglobalshortcut5 PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/core"
    "${CMAKE_CURRENT_SOURCE_DIR}/gui") # TODO: use properties
target_link_libraries(qxtglobalshortcut5 ${qxtglobalshortcut5_LIBS})


