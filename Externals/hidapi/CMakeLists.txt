project(hidapi)

add_library(hidapi STATIC hidapi/hidapi.h)
target_include_directories(hidapi PUBLIC hidapi)

if(APPLE)
  target_sources(hidapi PRIVATE mac/hid.c)
elseif(WIN32)
  target_sources(hidapi PRIVATE windows/hid.c)
  target_link_libraries(hidapi PRIVATE winmm setupapi)
else()
  find_package(Libudev)
  if(CMAKE_SYSTEM_NAME STREQUAL "Linux" AND LIBUDEV_FOUND)
    target_sources(hidapi PRIVATE linux/hid.c)
    target_link_libraries(hidapi PRIVATE udev)
  else()
    target_sources(hidapi PRIVATE libusb/hid.c)
  endif()
endif()

add_library(Hidapi::Hidapi ALIAS hidapi)
