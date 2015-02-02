CMAKE_MINIMUM_REQUIRED(VERSION 2.8.0)

PROJECT(libmodbus)

SET(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR})
SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR})

INCLUDE_DIRECTORIES(
	${PROJECT_SOURCE_DIR}
	${PROJECT_SOURCE_DIR}/src/win32
	)

SET(HEADER_FILES
    src/win32/config.h
    src/modbus.h
    src/modbus-private.h 
    src/modbus-rtu.h
    src/modbus-rtu-private.h
    src/modbus-tcp.h
    src/modbus-tcp-private.h
    src/modbus-version.h.in)

SET(SOURCE_FILES
    src/modbus.c
    src/modbus-tcp.c
    src/modbus-rtu.c
    src/modbus-data.c)

ADD_LIBRARY(modbus ${HEADER_FILES} ${SOURCE_FILES})

#------------------------------------------------------------------------------
add_custom_command(
    OUTPUT ${PROJECT_SOURCE_DIR}/src/win32/config.h
    COMMAND cd ${PROJECT_SOURCE_DIR}/src/win32 && cscript configure.js
	)
	
set_target_properties(modbus PROPERTIES LIBRARY_OUTPUT_DIRECTORY d:/tmp )
set_target_properties(modbus PROPERTIES LIBRARY_OUTPUT_DIRECTORY_DEBUG d:/tmp )
set_target_properties(modbus PROPERTIES LIBRARY_OUTPUT_DIRECTORY_RELEASE d:/tmp )
    
#------------------------------------------------------------------------------

INSTALL(FILES src/modbus.h DESTINATION include/modbus)
INSTALL(FILES src/modbus-rtu.h DESTINATION include/modbus)
INSTALL(FILES src/modbus-tcp.h DESTINATION include/modbus)
INSTALL(FILES src/modbus-version.h DESTINATION include/modbus)

INSTALL(TARGETS modbus DESTINATION lib)

ADD_CUSTOM_TARGET(uninstall @echo uninstall package)
    
ADD_CUSTOM_COMMAND(
    COMMENT "uninstall package"
    COMMAND xargs ARGS rm < install_manifest.txt
    COMMAND rm ARGS -r ${CMAKE_INSTALL_PREFIX}/include/vcore
    TARGET  uninstall)
