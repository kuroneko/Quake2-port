add_library(ctf MODULE
        g_ai.c
        g_chase.c
        g_cmds.c
        g_combat.c
        g_ctf.c
        g_func.c
        g_items.c
        g_main.c
        g_misc.c
        g_monster.c
        g_phys.c
        g_save.c
        g_spawn.c
        g_svcmds.c
        g_target.c
        g_trigger.c
        g_utils.c
        g_weapon.c
        m_move.c
        p_client.c
        p_hud.c
        p_menu.c
        p_trail.c
        p_view.c
        p_weapon.c
        q_shared.c)
target_compile_definitions(ctf PUBLIC ${QUAKE2_BUILD_DEFS})
if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    target_sources(ctf PRIVATE ctf.def)
endif()
set_target_properties(ctf PROPERTIES OUTPUT_NAME "game${QUAKE2_PLATFORM_SUFFIX}")
install(TARGETS ctf LIBRARY DESTINATION ctf)