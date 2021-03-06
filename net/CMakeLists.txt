CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

INCLUDE(${PROJECT_SOURCE_DIR}/CMakeCommon)

INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR})

AUX_SOURCE_DIRECTORY(. ANANAS_SRC)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    REMOVE(${ANANAS_SRC} Kqueue.cc Kqueue.h)
ELSEIF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    REMOVE(${ANANAS_SRC} Epoller.cc Epoller.h)
ENDIF()


ADD_LIBRARY(ananas_net ${ANANAS_SRC})
ADD_DEPENDENCIES(ananas_net ananas_util)
TARGET_LINK_LIBRARIES(ananas_net; ananas_util; pthread)
SET_TARGET_PROPERTIES(ananas_net PROPERTIES LINKER_LANGUAGE CXX)


INSTALL(TARGETS ananas_net DESTINATION lib)
set(HEADERS
    Application.h
    Connection.h
    EventLoop.h
    PipeChannel.h
    Poller.h
    Socket.h
    Typedefs.h
   )

INSTALL(FILES ${HEADERS} DESTINATION include/ananas/net)

