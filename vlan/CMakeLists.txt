cmake_minimum_required(VERSION 3.0)

# Service
project (vlan)

#check if conan is already run outside of project
if (NOT DEFINED CONAN_DEPENDENCIES)
  include(${CMAKE_CURRENT_BINARY_DIR}/conanbuildinfo.cmake OPTIONAL RESULT_VARIABLE HAS_CONAN)
  if (NOT HAS_CONAN)
    message(FATAL_ERROR "missing conanbuildinfo.cmake did you forget to run conan install ?")
  endif()
endif()
conan_basic_setup()

include(os)

set(SOURCES
    service.cpp # ...add more here
)

os_add_executable(vlan_service "VLAN example" ${SOURCES})

if ("$ENV{PLATFORM}" STREQUAL "x86_solo5")
  os_add_drivers(vlan_service
      solo5net
    )
else()
  os_add_drivers(vlan_service
    virtionet
    vmxnet3
    e1000
    boot_logger
  )
endif()

os_add_stdout(vlan_service default_stdout)
os_add_plugins(vlan_service autoconf)
