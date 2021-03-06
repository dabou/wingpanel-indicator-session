find_package (PkgConfig)

# Add all your dependencies to the list below
pkg_check_modules (DEPS REQUIRED gthread-2.0 gtk+-3.0 gio-2.0 accountsservice wingpanel-2.0 granite)

add_definitions (${DEPS_CFLAGS})
link_directories (${DEPS_LIBRARY_DIRS})

find_package (Vala REQUIRED)
include (ValaVersion)
ensure_vala_version ("0.22.0" MINIMUM)

include (ValaPrecompile)
# Add all your vala files and requires packages to the List below to include them in the build
vala_precompile (VALA_C ${CMAKE_PROJECT_NAME}
  Indicator.vala
  Widgets/UserBox.vala
  Widgets/UserListBox.vala
  Widgets/EndSessionDialog.vala
  Services/UserManager.vala
  Services/DbusInterfaces.vala
  ${CMAKE_CURRENT_BINARY_DIR}/config.vala
PACKAGES
  gio-2.0
  gtk+-3.0
  wingpanel-2.0
  granite
  AccountsService-1.0
OPTIONS
  --thread
)

add_library (${CMAKE_PROJECT_NAME} MODULE ${VALA_C})
target_link_libraries(${CMAKE_PROJECT_NAME} ${DEPS_LIBRARIES})

# Installation
install (TARGETS ${CMAKE_PROJECT_NAME} DESTINATION ${PKGDATADIR})
