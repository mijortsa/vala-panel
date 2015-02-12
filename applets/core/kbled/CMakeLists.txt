set(PLUGIN_NAME kbled)
vala_precompile(VALA_C ${PLUGIN_NAME}
    ${PLUGIN_NAME}.vala
PACKAGES
    ${CORE_PACKAGES}
    vala-panel
OPTIONS
    --vapidir=${CMAKE_SOURCE_DIR}/vapi
    --vapidir=${CMAKE_BINARY_DIR}/lib
    --vapidir=${CMAKE_BINARY_DIR}/vapi
    --target-glib=2.40
    --gresources=${CMAKE_CURRENT_SOURCE_DIR}/kbled.gresource.xml
    --thread
)
#####
# Plugin Library
#####
INCLUDE(GResource)
glib_compile_resources(RESOURCES
	source
    	kbled.gresource.xml
)

# Build library for plugin
set (LIBS ${CORE_LIBRARIES} -lm)
set (LIB_PATHS ${CORE_LIBRARY_DIRS})
link_directories (${LIB_PATHS})

add_library (${PLUGIN_NAME} MODULE
    ${VALA_C}
    ${RESOURCES}
)

target_link_libraries (${PLUGIN_NAME} ${LIBS})

# Install plugin stuffs
install(TARGETS ${PLUGIN_NAME} DESTINATION ${PLUGINS_DIRECTORY})
install(FILES ${PLUGIN_NAME}.plugin DESTINATION ${PLUGINS_DIRECTORY})