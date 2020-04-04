# Common source files used by absolutely everything:

add_library(common STATIC EXCLUDE_FROM_ALL
    crispy.cpp             crispy.hpp
    i_main.cpp
    i_system.cpp           i_system.hpp
    m_argv.cpp             m_argv.hpp
    m_misc.cpp             m_misc.hpp
)

target_include_directories(common PUBLIC
    "${PROJECT_BINARY_DIR}"
    "${PROJECT_SOURCE_DIR}/utils"
    "${CMAKE_CURRENT_SOURCE_DIR}"
)
target_link_libraries(common PUBLIC
    SDL2::SDL2
)

# Dedicated server (crispy-server):

option(BUILD_SERVER "Build server" ON)
if(BUILD_SERVER)
    add_executable(server WIN32
        d_dedicated.cpp
        d_iwad.cpp            d_iwad.hpp
        d_mode.cpp            d_mode.hpp
        deh_str.cpp           deh_str.hpp
        i_timer.cpp           i_timer.hpp
        m_config.cpp          m_config.hpp
        net_common.cpp        net_common.hpp
        net_dedicated.cpp     net_dedicated.hpp
        net_io.cpp            net_io.hpp
        net_packet.cpp        net_packet.hpp
        net_sdl.cpp           net_sdl.hpp
        net_query.cpp         net_query.hpp
        net_server.cpp        net_server.hpp
        net_structrw.cpp      net_structrw.hpp
        z_native.cpp          z_zone.hpp ../utils/lump.hpp
    )

    target_link_libraries(server
        common
        SDL2::net
    )

    set_target_properties(server
        PROPERTIES
            OUTPUT_NAME "${PROGRAM_PREFIX}server"
    )
endif()

# Source files used by the game binaries (chocolate-doom, etc.)
add_library(gamecommon STATIC EXCLUDE_FROM_ALL
    aes_prng.cpp          aes_prng.hpp
    d_event.cpp           d_event.hpp
                          doomkeys.hpp
                          doomtype.hpp
    d_iwad.cpp            d_iwad.hpp
    d_loop.cpp            d_loop.hpp
    d_mode.cpp            d_mode.hpp
                          d_ticcmd.hpp
    deh_str.cpp           deh_str.hpp
    gusconf.cpp           gusconf.hpp
    i_cdmus.cpp           i_cdmus.hpp
    i_endoom.cpp          i_endoom.hpp
    i_glob.cpp            i_glob.hpp
    i_input.cpp           i_input.hpp
    i_joystick.cpp        i_joystick.hpp
                          i_swap.hpp
    i_midipipe.cpp        i_midipipe.hpp
    i_musicpack.cpp
    i_oplmusic.cpp
    i_pcsound.cpp
    i_sdlmusic.cpp
    i_sdlsound.cpp
    i_sound.cpp           i_sound.hpp
    i_timer.cpp           i_timer.hpp
    i_video.cpp           i_video.hpp
    i_videohr.cpp         i_videohr.hpp
    midifile.cpp          midifile.hpp
    mus2mid.cpp           mus2mid.hpp
    m_bbox.cpp            m_bbox.hpp
    m_cheat.cpp           m_cheat.hpp
    m_config.cpp          m_config.hpp
    m_controls.cpp        m_controls.hpp
    m_fixed.cpp           m_fixed.hpp
    net_client.cpp        net_client.hpp
    net_common.cpp        net_common.hpp
    net_dedicated.cpp     net_dedicated.hpp
    net_defs.hpp
    net_gui.cpp           net_gui.hpp
    net_io.cpp            net_io.hpp
    net_loop.cpp          net_loop.hpp
    net_packet.cpp        net_packet.hpp
    net_petname.cpp       net_petname.hpp
    net_query.cpp         net_query.hpp
    net_sdl.cpp           net_sdl.hpp
    net_server.cpp        net_server.hpp
    net_structrw.cpp      net_structrw.hpp
    sha1.cpp              sha1.hpp
    memio.cpp             memio.hpp
    tables.cpp            tables.hpp
    v_diskicon.cpp        v_diskicon.hpp
    v_video.cpp           v_video.hpp
                          v_patch.hpp
    v_trans.cpp           v_trans.hpp
    w_checksum.cpp        w_checksum.hpp
    w_main.cpp            w_main.hpp
    w_wad.cpp             w_wad.hpp
    w_file.cpp            w_file.hpp
    w_file_stdc.cpp
    w_file_posix.cpp
    w_file_win32.cpp
    w_merge.cpp           w_merge.hpp
    z_zone.cpp            z_zone.hpp
)

if(MSVC)
    target_sources(gamecommon PRIVATE
         "../win32/win_opendir.cpp" "../win32/win_opendir.hpp"
     )
endif()

target_include_directories(gamecommon INTERFACE
    "${CMAKE_CURRENT_SOURCE_DIR}"
)

target_link_libraries(gamecommon PUBLIC
    common
    SDL2::mixer
    SDL2::net
    textscreen
    pcsound
    opl
)
if(SAMPLERATE_FOUND)
    target_link_libraries(gamecommon PUBLIC samplerate::samplerate)
endif()
if(PNG_FOUND)
    target_link_libraries(gamecommon PUBLIC PNG::PNG)
endif()

# Some games support dehacked patches, some don't:
add_library(gamecommon_dehacked STATIC
    deh_defs.hpp
    deh_io.cpp            deh_io.hpp
    deh_main.cpp          deh_main.hpp
    deh_mapping.cpp       deh_mapping.hpp
    deh_text.cpp
)
target_link_libraries(gamecommon_dehacked PUBLIC
    gamecommon
)

option(BUILD_DOOM "Build doom" ON)
if(BUILD_DOOM)
    add_subdirectory(doom)
endif()

option(BUILD_HERETIC "Build heretic" ON)
if(BUILD_HERETIC)
    add_subdirectory(heretic)
endif()

option(BUILD_HEXEN "Build hexen" ON)
if(BUILD_HEXEN)
    add_subdirectory(hexen)
endif()

option(BUILD_STRIFE "Build strife" ON)
if(BUILD_STRIFE)
    add_subdirectory(strife)
endif()

# Source files needed for chocolate-setup:

option(BUILD_SETUP "Build setup" ON)
if(BUILD_SETUP)
    add_library(setupcommon
        deh_str.cpp           deh_str.hpp
        d_mode.cpp            d_mode.hpp
        d_iwad.cpp            d_iwad.hpp
        i_timer.cpp           i_timer.hpp
        m_config.cpp          m_config.hpp
        m_controls.cpp        m_controls.hpp
        net_io.cpp            net_io.hpp
        net_packet.cpp        net_packet.hpp
        net_petname.cpp       net_petname.hpp
        net_sdl.cpp           net_sdl.hpp
        net_query.cpp         net_query.hpp
        net_structrw.cpp      net_structrw.hpp
        z_native.cpp          z_zone.hpp
    )

    target_link_libraries(setupcommon PUBLIC
        common
    )

    add_subdirectory(setup)
endif()

add_executable(midiread
    deh_str.cpp
    d_iwad.cpp
    i_system.cpp
    midifile.cpp
    m_argv.cpp
    m_config.cpp
    m_misc.cpp
    z_native.cpp
)
target_compile_definitions(midiread PRIVATE
    "TEST"
)
target_include_directories(midiread PRIVATE
    "${PROJECT_BINARY_DIR}"
)
target_link_libraries(midiread PRIVATE
    SDL2::SDL2
)

add_executable(mus2mid
    mus2mid.cpp
    memio.cpp
    z_native.cpp
    i_system.cpp
    m_argv.cpp
    m_misc.cpp
    d_iwad.cpp
    deh_str.cpp
    m_config.cpp
)
target_compile_definitions(mus2mid PRIVATE
    "STANDALONE"
)
target_include_directories(mus2mid PRIVATE
    "${PROJECT_BINARY_DIR}"
)
target_link_libraries(mus2mid PRIVATE
    SDL2::SDL2
)
