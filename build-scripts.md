# Build Scripts

QDK supports four different types of build scripts, one that is called once before the actual build process isstarted, one that is called before each build, one that is called after each build, and finally, one that is calledonce after the build process is finished. If a script returns a non-zero value then the build process isinterrupted and the qbuild application exits with an error message. The build scripts should use the returncommand to return the status and not the exit command. If exit is used then not only the build script exits,but the build process itself, too.

The script specified with --setup \(or the **QDK\_SETUP** variable\) could be useful for any global changes beforethe build process is started. When the defined setup script is called is has access to all the QDK prefixedvariables, but many of them will not be assigned any values, yet.

Also, when the script is run qbuild has not checked that the configuration file exists. In other words, it wouldfor example be possible to generate a configuration file in the script and then, if necessary, update the **QDK\_QPKG\_CONFIG** to specify the new location. Similar to **QDK\_QPKG\_CONFIG** the **QDK\_PACKAGE\_ROUTINES** variable is also still unchecked at the time when the setup script is run, so thisfile could also be generated in the script or the location to a generic file could be specified.

If a QPKG configuration file has been created by the setup script or it has been verified that there is a validQPKG configuration file at the location specified in **$QDK\_QPKG\_CONFIG** then the values in theconfiguration file can be modified by the setup script using one of qbuild's support functions,

## edit\_qpkg\_config FIELD VALUE \[location of QPKG configuration file\]

If the location of the file isn't specified then the default location specified in $QDK\_QPKG\_CONFIG is used.

The next script is the pre-build script, specified with the --pre-build option or by using the **QDK\_PRE\_BUILD** variable. This script could be useful to make any package specific changes before each build, for examplearchitecture specific handling like building a binary for the current architecture or include a pre-packagedarchive for the current architecture..

When the pre-build script is run the $QDK\_QPKG\_CONFIG and $QDK\_PACKAGE\_ROUTINES have beenchecked to make sure they exist. The QDK\_BUILD\_ARCH variable have been assigned values if not already assigned.

The pre-build script is called with two arguments, the first one is set to the architecture for which the QPKGfile is about to be built, and the second argument is set to the location of the architecture specific files. Forthe generic build the arguments are empty.

This script has access to the same variables as the setup script and also all the QPKG prefixed variables fromthe QPKG configuration file. The QPKG prefixed variables can be changed using edit\_qpkg\_config .

Note that if the QDK\_DATA\_FILE variable has been assigned a value for an existing file then the steps later inthe build process to create a data package are skipped and the file specified in $QDK\_DATA\_FILE is usedinstead \(although renamed to one of data.tar.gz, data.tar.bz2, or data.tar.7z when included in the QPKGpackage; the name depends on the compression method used for the specified tar archive in$QDK\_DATA\_FILE \).

If QDK\_BUILD\_VERSION was specified in the user configuration file, in the setup or pre-build scripts, or if--build-version option was specified on the command line then the QPKG configuration file is updated to setthe QPKG\_VER field to the specified version before the QPKG package is built.

After the QPKG package has been built for the selected architecture \(or generic\) the post-build script iscalled, specified with the --post-build option or by using the QDK\_POST\_BUILD definition. It has access tothe same variables as the pre-build script and for any file operations on the built QPKG package, the file canbe found in $QDK\_BUILD\_DIR with the name $QDK\_QPKG\_FILE .

The post-build script also receives the architecture and location of architecture specific files as arguments\(although the location of the files might be of less use to this script\).

After the post-build is finished the build continues for the next architecture or if nothing more to build thelast script, teardown, is called.

The teardown script, specified with the --teardown option or by using the QDK\_TEARDOWN definition, hasaccess to the same variables as all the previous scripts, although many of them would be of limited useconsidering that this script performs the last few actions before the build process exits.

All the scripts can use the following support functions for error messages, warning messages, normalmessages, verbose messages, and debug

messageserr\_msg MSG

warn\_msg MSG

msg MSG

verbose\_msg MSG

debug\_msg MSG

Actual output depends on $QDK\_VERBOSE and $QDK\_STRICT settings. A call to err\_msg will always resultin a termination of the build process and if $QDK\_STRICT is set to TRUE then warn\_msg gives the same result.

