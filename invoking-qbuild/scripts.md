# Scripts

One of qbuild's most powerful features is the script support. It makes it possible to run scripts at certainphases in the build process. The scripts can run any available shell commands and have access to both QPKGand QDK defined variables. This was discussed in greater details in a previous chapter.

The --setup option can be used to run a specified script to setup the build environment. It is called oncebefore the build process is initiated. To clean up after all builds are finished the --teardown option can beused to specify the script to run. Called once after all builds are completed.

To control the build of a QPKG package the --pre-build option can be used. The specified script is run beforethe build process is started and is called before each and every build. First argument contains the architecture\(one of arm-x09, arm-x19, x86, and x86\_64\) and the second argument contains the location of thearchitecture specific code. For the generic build the arguments are empty. It is also possible to specify ascript that shall be run after the build process is finished with the --post-build option. It is called after eachand every build with the same kind of arguments as the pre-build script.

