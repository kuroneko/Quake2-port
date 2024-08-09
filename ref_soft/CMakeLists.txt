add_library(ref_soft MODULE
        r_aclip.c
        r_alias.c
        r_bsp.c
        r_draw.c
        r_edge.c
        r_image.c
        r_light.c
        r_main.c
        r_misc.c
        r_model.c
        r_part.c
        r_poly.c
        r_polyse.c
        r_rast.c
        r_scan.c
        r_sprite.c
        r_surf.c
        ../game/q_shared.c)
target_compile_definitions(ref_soft PUBLIC ${QUAKE2_BUILD_DEFS})
if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    target_sources(ref_soft PRIVATE
            ../win32/q_shwin.c
            ../win32/rw_imp.c
            ../win32/rw_ddraw.c
            ../win32/rw_dib.c
            ref_soft.def)
    target_link_libraries(ref_soft PUBLIC winmm)
endif()
install(TARGETS ref_soft LIBRARY DESTINATION .)
