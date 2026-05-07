# OpenUTV Dependencies

This repository holds the automated Windows dependency build pipeline for OpenUTV using `vcpkg`.
It generates the `OpenUTVDeps` MSI installer that the primary OpenUTV pipeline consumes.

## Versioning

This repository uses **Source-Driven Semantic Versioning**.
The version of the generated MSI and the GitHub Release tag is defined directly in `CMakeLists.txt`:

```cmake
project(OpenUTVDependencies VERSION 26.1)
```

**⚠️ IMPORTANT NOTE FOR MAINTAINERS:**
Before merging significant dependency updates to the `main` branch, you **MUST increment the version** in `CMakeLists.txt` (e.g., from `26.1` to `26.2`).
If you run the GitHub Action without incrementing the version, it will simply overwrite (clobber) the existing release MSI for that version. This is useful for hotfixes or rebuilding the exact same version, but should generally be avoided for new features to ensure reproducible builds downstream.
