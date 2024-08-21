cmake_minimum_required(VERSION 3.5)
project(kio-mtp)

set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU" OR
        "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    add_compile_options("$<$<CONFIG:DEBUG>:-Wall;-Wextra;-pedantic>")
endif()

set( CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules )

find_package(KDE4 REQUIRED)
find_package(Mtp)

include(KDE4Defaults)

#add_definitions(-DKDE_DEFAULT_DEBUG_AREA=7999)
add_definitions(-DQT_NO_CAST_FROM_ASCII)
add_definitions(-DKDE_NO_DEPRECATED)

set( kio_mtp_PART_SRCS
     devicecache.cpp
     filecache.cpp
     kio_mtp.cpp
     kio_mtp_helpers.cpp
)

include_directories(
    ${KDE4_INCLUDES}
    ${CMAKE_CURRENT_BINARY_DIR}
)

kde4_add_plugin( kio_mtp ${kio_mtp_PART_SRCS} )
target_link_libraries( kio_mtp ${KDE4_KIO_LIBRARY} ${MTP_LIBRARIES} ${KDE4_SOLID_LIBS} )

install( TARGETS kio_mtp DESTINATION ${PLUGIN_INSTALL_DIR} )


########### install files ###############

install( FILES mtp-network.desktop  DESTINATION  ${DATA_INSTALL_DIR}/konqueror/dirtree/remote )
install( FILES solid_mtp.desktop  DESTINATION  ${DATA_INSTALL_DIR}/solid/actions )
install( FILES mtp.protocol  DESTINATION  ${SERVICES_INSTALL_DIR} )
install( FILES mtp-network.desktop  DESTINATION  ${DATA_INSTALL_DIR}/remoteview )
