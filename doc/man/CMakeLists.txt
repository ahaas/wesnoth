set(MANPAGES)
if(ENABLE_GAME)
	set(MANPAGES ${MANPAGES} wesnoth.6)
endif()
if(ENABLE_SERVER)
	set(MANPAGES ${MANPAGES} wesnothd.6)
endif()

set(ALL_MANPAGES wesnoth.6 wesnothd.6)

foreach(MANPAGE ${MANPAGES})
	install(FILES ${MANPAGE} DESTINATION "${CMAKE_INSTALL_MANDIR}/man6/")

	if(ENABLE_NLS)
		foreach(LANG ${LINGUAS})
			install(FILES ${LANG}/${MANPAGE} DESTINATION "${CMAKE_INSTALL_MANDIR}/${LANG}/man6/" OPTIONAL)
		endforeach(LANG ${LINGUAS})
	endif(ENABLE_NLS)
endforeach(MANPAGE ${MANPAGES})

if(ENABLE_POT_UPDATE_TARGET)
	add_custom_target(update-po4a-man
		COMMAND ${CMAKE_COMMAND}
			-DPROJECT_SOURCE_DIR="${PROJECT_SOURCE_DIR}"
			-DMANPAGES="${ALL_MANPAGES}"
			-P "${CMAKE_SOURCE_DIR}/cmake/po4a-man.cmake"
		DEPENDS
			${CMAKE_CURRENT_SOURCE_DIR}/wesnoth.6
			${CMAKE_CURRENT_SOURCE_DIR}/wesnothd.6
		WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
endif(ENABLE_POT_UPDATE_TARGET)
