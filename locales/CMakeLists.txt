# Check that people use the good file
if(NOT TOP_CMAKE_WAS_SOURCED)
	message(FATAL_ERROR "
	You did not 'cmake' the good CMakeLists.txt file. Use the one in the top dir.
	It is advice to delete all wrongly generated cmake stuff => CMakeFiles & CMakeCache.txt")
endif(NOT TOP_CMAKE_WAS_SOURCED)

if (GETTEXT_FOUND)
	include(Translation)

	file (GLOB_RECURSE PO_ICO_FILES ${CMAKE_SOURCE_DIR}}/locales  pcsx2_Iconized.po)
	file (GLOB_RECURSE PO_MAIN_FILES ${CMAKE_SOURCE_DIR}/locales  pcsx2_Main.po)

	# Macro to compile po file and install them
	GETTEXT_CREATE_TRANSLATIONS_PCSX2(${CMAKE_SOURCE_DIR}/locales/templates/pcsx2_Iconized.pot ALL ${PO_ICO_FILES})
	GETTEXT_CREATE_TRANSLATIONS_PCSX2(${CMAKE_SOURCE_DIR}/locales/templates/pcsx2_Main.pot     ALL ${PO_MAIN_FILES})

endif (GETTEXT_FOUND)
