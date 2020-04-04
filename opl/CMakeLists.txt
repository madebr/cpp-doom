add_library(opl STATIC EXCLUDE_FROM_ALL
    opl_internal.hpp
    opl.cpp             opl.hpp
    opl_linux.cpp
    opl_obsd.cpp
    opl_queue.cpp       opl_queue.hpp
    opl_sdl.cpp
    opl_timer.cpp       opl_timer.hpp
    opl_win32.cpp
    ioperm_sys.cpp      ioperm_sys.hpp
    opl3.cpp            opl3.hpp
)

target_include_directories(opl
    INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}"
    PRIVATE "${PROJECT_BINARY_DIR}"
)
target_link_libraries(opl PUBLIC
    SDL2::mixer
)
