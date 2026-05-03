cmake_minimum_required(VERSION 3.15)
project(OpenUTVDependencies VERSION 2026.1.0)

# Install the raw vcpkg export
install(DIRECTORY ${CMAKE_SOURCE_DIR}/exported_deps/utv-deps-windows-x64/ DESTINATION . COMPONENT deps)

set(CPACK_GENERATOR "WIX")
set(CPACK_PACKAGE_NAME "OpenUTVDeps")
set(CPACK_PACKAGE_VENDOR "OpenUTV")
set(CPACK_WIX_UPGRADE_GUID "D2305530-58FF-4054-94E6-37D35E445EFA")
set(CPACK_WIX_INSTALL_SCOPE "perUser")

# Add the environment variable using WIX patch
file(WRITE ${CMAKE_BINARY_DIR}/wix_patch.xml "
<CPackWiXFragment Id=\"CM_CP_deps\">
  <Environment Id=\"UpdateOpenUTVDepsRoot\" Action=\"set\" Name=\"OPENUTV_DEPS_ROOT\" Value=\"[INSTALLFOLDER]\" System=\"no\" Permanent=\"no\" />
</CPackWiXFragment>
")
set(CPACK_WIX_PATCH_FILE "${CMAKE_BINARY_DIR}/wix_patch.xml")

include(CPack)
