set (MODULE_NAME anttweakbar)

# Get all compilation units.
file (GLOB ${MODULE_NAME}_SRCS 
	LoadOGL.cpp
	LoadOGLCore.cpp
	TwBar.cpp
	TwColors.cpp
	TwDirect3D10.cpp
	TwDirect3D11.cpp
	TwDirect3D9.cpp
	TwEventGLFW.c
	TwEventGLFW3.c
	TwEventGLUT.c
	TwEventSDL.c
	TwEventSDL12.c
	TwEventSDL13.c
	TwEventSFML.cpp
	TwEventWin.c
	TwFonts.cpp
	TwMgr.cpp
	TwOpenGL.cpp
	TwOpenGLCore.cpp
	TwPrecomp.cpp
)

# Create the library.
add_library (${MODULE_NAME} ${${MODULE_NAME}_SRCS})

if (MSVC)
	if(CMAKE_SIZEOF_VOID_P EQUAL 8) 
	    set(TARGET_ARCHITECTURE 64) 
	else() 
	    set(TARGET_ARCHITECTURE 32) 
	endif() 

	add_custom_command(
		TARGET ${MODULE_NAME}
		PRE_BUILD
		COMMAND mkdir ARGS \"${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}${TARGET_ARCHITECTURE}\"
		COMMAND fxc ARGS /T vs_4_0_level_9_1 /E LineRectVS /Fh ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}${TARGET_ARCHITECTURE}/TwDirect3D11_LineRectVS.h ${CMAKE_CURRENT_LIST_DIR}/TwDirect3D11.hlsl
		COMMAND fxc ARGS /T vs_4_0_level_9_1 /E LineRectCstColorVS /Fh ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}${TARGET_ARCHITECTURE}/TwDirect3D11_LineRectCstColorVS.h ${CMAKE_CURRENT_LIST_DIR}/TwDirect3D11.hlsl
		COMMAND fxc ARGS /T ps_4_0_level_9_1 /E LineRectPS /Fh ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}${TARGET_ARCHITECTURE}/TwDirect3D11_LineRectPS.h ${CMAKE_CURRENT_LIST_DIR}/TwDirect3D11.hlsl
		COMMAND fxc ARGS /T vs_4_0_level_9_1 /E TextVS /Fh ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}${TARGET_ARCHITECTURE}/TwDirect3D11_TextVS.h ${CMAKE_CURRENT_LIST_DIR}/TwDirect3D11.hlsl
		COMMAND fxc ARGS /T vs_4_0_level_9_1 /E TextCstColorVS /Fh ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}${TARGET_ARCHITECTURE}/TwDirect3D11_TextCstColorVS.h ${CMAKE_CURRENT_LIST_DIR}/TwDirect3D11.hlsl
		COMMAND fxc ARGS /T ps_4_0_level_9_1 /E TextPS /Fh ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}${TARGET_ARCHITECTURE}/TwDirect3D11_TextPS.h ${CMAKE_CURRENT_LIST_DIR}/TwDirect3D11.hlsl
	)
	include_directories(${CMAKE_BINARY_DIR})
endif (MSVC)

install (TARGETS ${MODULE_NAME} 
		ARCHIVE DESTINATION lib 
		LIBRARY DESTINATION lib)