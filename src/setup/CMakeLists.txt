add_library(setup STATIC
    compatibility.cpp       compatibility.hpp
    display.cpp             display.hpp
    joystick.cpp            joystick.hpp
    keyboard.cpp            keyboard.hpp
    mainmenu.cpp
    mode.cpp                mode.hpp
    mouse.cpp               mouse.hpp
    multiplayer.cpp         multiplayer.hpp
    sound.cpp               sound.hpp
    execute.cpp             execute.hpp
    txt_joyaxis.cpp         txt_joyaxis.hpp
    txt_joybinput.cpp       txt_joybinput.hpp
    txt_keyinput.cpp        txt_keyinput.hpp
    txt_mouseinput.cpp      txt_mouseinput.hpp
)

if(WIN32)
    target_sources(setup PRIVATE
        "${CMAKE_CURRENT_BINARY_DIR}/../setup-res.rc"
    )
endif()

target_link_libraries(setup PRIVATE
    setupcommon
)

set_target_properties(setup PROPERTIES
    OUTPUT_NAME "${PROGRAM_PREFIX}setup"
)

if(MSVC)
    set_target_properties(setup PROPERTIES
        LINK_FLAGS "/MANIFEST:NO"
    )
endif()
