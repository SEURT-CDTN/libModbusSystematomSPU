cmake_minimum_required(VERSION 3.12)

project(
  libModbusSystematomSPU
  VERSION 0.0.0
  DESCRIPTION "Comunicate with the SystemAtom SPU using MODBUS-RTU"
  HOMEPAGE_URL "https://github.com/SEURT-CDTN/libModbusSystematomSPU"
  LANGUAGES CXX)

find_package(PkgConfig REQUIRED)

pkg_search_module(Modbus REQUIRED IMPORTED_TARGET libmodbus)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_C_STANDARD_INCLUDE_DIRECTORIES
    ${CMAKE_C_IMPLICIT_INCLUDE_DIRECTORIES})

set(LIBMODBUSSYSTEMATOMSPU_SRC src/libModbusSystematomSPU.cpp)

add_library(modbusSystematomSPU ${LIBMODBUSSYSTEMATOMSPU_SRC})
add_library(modbusSystematomSPU::modbusSystematomSPU ALIAS modbusSystematomSPU)

target_include_directories(
  modbusSystematomSPU PUBLIC ${PROJECT_SOURCE_DIR}/include
                             ${MODBUS_INCLUDE_DIRS})
target_link_libraries(modbusSystematomSPU PUBLIC PkgConfig::Modbus)
target_compile_options(modbusSystematomSPU PUBLIC ${MODBUS_CFLAGS_OTHER})

add_executable(test src/test.cpp)
target_include_directories(test PRIVATE ${PROJECT_SOURCE_DIR}/include)
target_link_libraries(test PRIVATE modbusSystematomSPU)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/
        DESTINATION ${CMAKE_INSTALL_PREFIX}/include/libModbusSystematomSPU/)
install(
  TARGETS modbusSystematomSPU
  LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
  ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
  RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)

install(TARGETS test RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)