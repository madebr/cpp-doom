add_library(pcsound STATIC EXCLUDE_FROM_ALL
    pcsound.cpp             pcsound.hpp
    pcsound_bsd.cpp
    pcsound_sdl.cpp
    pcsound_linux.cpp
    pcsound_win32.cpp
                            pcsound_internal.hpp
)
target_include_directories(pcsound
    INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}"
    PUBLIC "${PROJECT_BINARY_DIR}"
)
target_link_libraries(pcsound PUBLIC
    SDL2::mixer
)
