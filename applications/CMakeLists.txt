project(segyio-apps)

if( NOT HAVE_GETOPT_H OR NOT HAVE_GETOPT_LONG )
    message(WARNING "Could not find getopt. Not building applications.")
    return ()
endif ()

if (NOT MSVC)
    set(CMAKE_C_FLAGS "-std=c99 ${CMAKE_C_FLAGS}")
endif()

add_library(apputils STATIC apputils.c)
target_link_libraries(apputils segyio)
target_compile_definitions(apputils PUBLIC
    -Dsegyio_MAJOR=${segyio_MAJOR}
    -Dsegyio_MINOR=${segyio_MINOR}
)

add_executable(segyinfo segyinfo.c)
target_link_libraries(segyinfo segyio)

add_executable(segyinspect segyinspect.c)
target_link_libraries(segyinspect segyio)

add_executable(segyio-cath segyio-cath.c)
target_link_libraries(segyio-cath segyio apputils)

add_executable(segyio-catb segyio-catb.c)
target_link_libraries(segyio-catb segyio apputils)

add_executable(segyio-catr segyio-catr.c)
target_link_libraries(segyio-catr segyio apputils)

add_executable(segyio-crop segyio-crop.c)
target_link_libraries(segyio-crop segyio apputils)

install(TARGETS segyinfo DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)
install(TARGETS segyinspect DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)

install(TARGETS segyio-cath DESTINATION ${CMAKE_INSTALL_BINDIR})
install(TARGETS segyio-catb DESTINATION ${CMAKE_INSTALL_BINDIR})
install(TARGETS segyio-catr DESTINATION ${CMAKE_INSTALL_BINDIR})
install(TARGETS segyio-crop DESTINATION ${CMAKE_INSTALL_BINDIR})

add_python_test(applications.catr test/catr.py)
add_python_test(applications.catb test/catb.py)
add_python_test(applications.cath test/cath.py)

