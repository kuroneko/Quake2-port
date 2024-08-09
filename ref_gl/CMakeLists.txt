add_library(ref_gl MODULE
        gl_draw.c
        gl_image.c
        gl_light.c
        gl_mesh.c
        gl_model.c
        gl_rmain.c
        gl_rmisc.c
        gl_rsurf.c
        gl_warp.c
        ../game/q_shared.c)
target_compile_definitions(ref_gl PUBLIC ${QUAKE2_BUILD_DEFS})
if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    target_sources(ref_gl PRIVATE
            ../win32/glw_imp.c
            ../win32/q_shwin.c
            ../win32/qgl_win.c
            ref_gl.def)
    target_link_libraries(ref_gl PUBLIC winmm)
endif()
install(TARGETS ref_gl LIBRARY DESTINATION .)
