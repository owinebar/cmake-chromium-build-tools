# cmake-chromium-build-tools
CMake modules to support using depot_tools and other chromium build infrastructure for importing and building third party dependencies in a project

The Chromium project (at least v8) uses gclient to maintain vendored external (possibly also chromium) project sources as subtrees of the source of a given project.   
The gclient configuration is also used to specify "hooks" that will, among other things, retrieve prebuilt binaries, particularly build tooling like the clang compiler,
using the "download_from_google_storage.py" script in some copy of depot_tools.  The depot_tools repo may be vendored into the source tree using gclient, or it might be 
an existing copy expected to be present on the developer's machine.

The goal of this project is to run gclient in a CMake module, intercept calls to the "download_from_google_storage.py" hook and create a cmake module that will use 
CMake procedures to locate equivalent tooling present on the developer's system, or retrieve the source and build the tool from scratch, or retrieve a prebuilt binary
from an alternate source trusted by the developer running this meta-conversion.  

One of the stumbling blocks of attempting to use a Chromium project as a dependency is that the build process of the projects, even when configured to produce shared libraries,
do not provide outputs useful for metadata of the type managed by tools like pkg-config.  One of the results of the CMake processing of a chromium dependency will be using
the gn build tool to retrieve the metadata, e.g. compiler and linker flags, needed to build objects that will link (and run) successfully with the libraries created
as part of the chromium dependency.
