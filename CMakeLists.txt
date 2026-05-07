cmake_minimum_required(VERSION 3.15)
project(OpenUTVDependencies VERSION 26.1)

# Install the raw vcpkg export
install(DIRECTORY ${CMAKE_SOURCE_DIR}/exported_deps/utv-deps-windows-x64/ DESTINATION . COMPONENT deps)

set(CPACK_GENERATOR "WIX")
set(CPACK_PACKAGE_NAME "OpenUTVDeps")
set(CPACK_PACKAGE_VENDOR "OpenUTV")
set(CPACK_WIX_UPGRADE_GUID "D2305530-58FF-4054-94E6-37D35E445EFA")
# We must NOT override the root folder ID, so CPack successfully anchors INSTALL_ROOT to ProgramFilesFolder (preventing WiX LGHT0231 GUID errors)

# Define a custom WIX source file to inject the Environment component directly using pure WiX
file(WRITE ${CMAKE_BINARY_DIR}/extra.wxs "<?xml version=\"1.0\" encoding=\"UTF-8\"?>
<Wix xmlns=\"http://schemas.microsoft.com/wix/2006/wi\">
    <Fragment>
        <DirectoryRef Id=\"INSTALL_ROOT\">
            <Component Id=\"EnvVarComponent\" Guid=\"B4E63C47-D611-4770-9856-F1439F68B610\">
                <Environment Id=\"UpdateOpenUTVDepsRoot\" Action=\"set\" Name=\"OPENUTV_DEPS_ROOT\" Value=\"[INSTALL_ROOT]\" System=\"yes\" Permanent=\"no\" />
            </Component>
        </DirectoryRef>
    </Fragment>
    <Fragment>
        <FeatureRef Id=\"ProductFeature\">
            <ComponentRef Id=\"EnvVarComponent\" />
        </FeatureRef>
    </Fragment>
</Wix>
")
set(CPACK_WIX_EXTRA_SOURCES "${CMAKE_BINARY_DIR}/extra.wxs")

include(CPack)
