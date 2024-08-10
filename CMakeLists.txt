cmake_minimum_required(VERSION 3.20)
project(Quake2 VERSION 3.21 LANGUAGES C)

message("Building Quake2 for ${CMAKE_SYSTEM_NAME} on ${CMAKE_SYSTEM_PROCESSOR}")
if(CMAKE_SYSTEM_NAME MATCHES Windows)
    # can't trust CMAKE_SYSTEM_PROCESSOR in this case
    if(CMAKE_SIZEOF_VOID_P EQUAL 8)
        set(QUAKE2_PLATFORM_SUFFIX "x64")
        set(QUAKE2_BUILD_DEFS C_ONLY)
        set(QUAKE2_CACHELINE_SIZE 64)
    else()
        set(QUAKE2_PLATFORM_SUFFIX "x86")
        # this is probably incorrect today, but matches the legacy behaviour
        set(QUAKE2_CACHELINE_SIZE 32)
    endif()
elseif(CMAKE_SYSTEM_NAME MATCHES Linux)
    set(QUAKE2_PLATFORM_SUFFIX "linux-${CMAKE_SYSTEM_PROCESSOR}")
    if(CMAKE_SIZEOF_VOID_P EQUAL 8)
        set(QUAKE2_CACHELINE_SIZE 64)
    else()
        # this is probably incorrect today, but matches the legacy behaviour
        set(QUAKE2_CACHELINE_SIZE 32)
    endif()
else()
    # this is probably incorrect today, but matches the legacy behaviour
    set(QUAKE2_CACHELINE_SIZE 32)
endif()
set(QUAKE2_BUILD_DEFS ${QUAKE2_BUILD_DEFS} QUAKE2_CACHELINE_SIZE=${QUAKE2_CACHELINE_SIZE})

if(CMAKE_C_BYTE_ORDER MATCHES BIG_ENDIAN)
    set(QUAKE2_BUILD_DEFS ${QUAKE2_BUILD_DEFS} QUAKE2_HOST_BIG_ENDIAN=1)
    message("Target host is Big Endian")
elseif(CMAKE_C_BYTE_ORDER MATCHES LITTLE_ENDIAN)
    set(QUAKE2_BUILD_DEFS ${QUAKE2_BUILD_DEFS} QUAKE2_HOST_LITTLE_ENDIAN=1)
    message("Target host is Little Endian")
else()
    message(FATAL_ERROR "Could not identify system endianness")
endif()

message("Target suffix is ${QUAKE2_PLATFORM_SUFFIX}")

add_executable(Quake2 WIN32
        client/cl_cin.c
        client/cl_ents.c
        client/cl_fx.c
        client/cl_input.c
        client/cl_inv.c
        client/cl_main.c
        client/cl_newfx.c
        client/cl_parse.c
        client/cl_pred.c
        client/cl_tent.c
        client/cl_scrn.c
        client/cl_view.c
        client/console.c
        client/keys.c
        client/menu.c
        client/snd_dma.c
        client/snd_mem.c
        client/snd_mix.c
        client/qmenu.c
        game/m_flash.c

        qcommon/cmd.c
        qcommon/cmodel.c
        qcommon/common.c
        qcommon/crc.c
        qcommon/cvar.c
        qcommon/files.c
        qcommon/md4.c
        qcommon/net_chan.c

        server/sv_ccmds.c
        server/sv_ents.c
        server/sv_game.c
        server/sv_init.c
        server/sv_main.c
        server/sv_send.c
        server/sv_user.c
        server/sv_world.c

        game/q_shared.c
        qcommon/pmove.c)
target_compile_definitions(Quake2
        PUBLIC QUAKE2_PLATFORM_SUFFIX="${QUAKE2_PLATFORM_SUFFIX}" ${QUAKE2_BUILD_DEFS})
install(TARGETS Quake2 RUNTIME DESTINATION ".")

if(NOT CMAKE_SYSTEM_NAME MATCHES "Windows")
# There was no dedicated server only binary on Windows
add_executable(q2ded
        qcommon/cmd.c
        qcommon/cmodel.c
        qcommon/common.c
        qcommon/crc.c
        qcommon/cvar.c
        qcommon/files.c
        qcommon/md4.c
        qcommon/net_chan.c

        server/sv_ccmds.c
        server/sv_ents.c
        server/sv_game.c
        server/sv_init.c
        server/sv_main.c
        server/sv_send.c
        server/sv_user.c
        server/sv_world.c

        game/q_shared.c
        qcommon/pmove.c

        null/cl_null.c
        null/cd_null.c)
target_compile_definitions(q2ded PUBLIC DEDICATED_ONLY ${QUAKE2_BUILD_DEFS})
install(TARGETS q2ded RUNTIME DESTINATION ".")
endif()

# platform specific parts
if(CMAKE_SYSTEM_NAME MATCHES "Linux")
    target_sources(Quake2 PUBLIC
            linux/cd_linux.c
            linux/q_shlinux.c
            linux/vid_menu.c
            linux/vid_so.c
            linux/snd_linux.c
            linux/sys_linux.c
            linux/glob.c
            linux/net_udp.c)

    target_sources(q2ded PUBLIC
            linux/q_shlinux.c
            linux/sys_linux.c
            linux/glob.c
            linux/net_udp.c)

elseif(CMAKE_SYSTEM_NAME MATCHES "Windows")
    target_sources(Quake2 PUBLIC
            win32/cd_win.c
            win32/q_shwin.c
            win32/vid_menu.c
            win32/vid_dll.c
            win32/in_win.c
            win32/snd_win.c
            win32/sys_win.c
            win32/conproc.c
            win32/net_wins.c
            win32/q2.rc)
    target_link_libraries(Quake2 PUBLIC
            winmm wsock32)
endif()

add_subdirectory(game)
add_subdirectory(ctf)

add_subdirectory(ref_gl)
add_subdirectory(ref_soft)