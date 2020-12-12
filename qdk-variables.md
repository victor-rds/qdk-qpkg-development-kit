# QDK Variables

To configure the build process it is possible to assign values to different QDK variables. All the QDKprefixed variables can be included in the system wide configuration file, \/etc\/config\/qdk.conf, or in the user'sconfiguration file, ~\/.qdkrc. The variables can also be included in the QPKG configuration file or buildscripts. Command line arguments override any variables in the user's configuration file, which in turnoverride any variables in the system wide configuration file. Variables specified in the QPKG configurationfile or in a build script can also override previous specified \(or default\) values.

## QDK\_VERSION

QDK version.

## QDK\_PATH

Path to where QDK is installed.

## QDK\_USER\_CONFIG\_FILE

Location of the user's configuration file. Default value is ~\/.qdkrc.

## QDK\_QPKG\_CONFIG

Location \\(or expected location\\) of the QPKG configuration file. Default value for this variable is a filenamed qpkg.cfg in the current directory.

## QDK\_PACKAGE\_ROUTINES

Location \\(or expected location\\) of file with package specific functions. Default value for this variable is afile named package\\_routines in the current directory.

## QDK\_SCRIPTS\_DIR

Location of directory with the script files that are used by qbuild. Default value is $QDK\\_PATH \/scripts.

## QDK\_TEMPLATE\_DIR

Location of the directory with the template files that should be used when creating a new buildenvironment using the --create-env option. Currently, generic versions of qpkg.cfg and package\\_routinesshall be located in the specified directory. Any other files and directories are optional. Default value is$QDK\\_PATH \/template.

## QDK\_INSTALL\_SCRIPT

Location of the generic installation script. That is, the script that is run when the QPKG is installed.Default value is $QDK\\_SCRIPTS\\_DIR \/qinstall.sh.

## QDK\_VERBOSE

Indicates qbuild's level of verbosity. 0 is quiet mode, 1 is normal mode, 2 is verbose mode, 3 is debugmode, and 4 is an extra verbose debug mode. Default value is 1 .

## QDK\_STRICT

Indicates if qbuild is run in strict mode or not. Default value is FALSE . In strict mode all warnings aretreated as errors.

## QDK\_FORCE\_CONFIG

When set to TRUE then qbuild ignores that specified configuration files are missing from the package.Can be used when the configuration files are created at installation of the QPKG package. Default isFALSE .

## QDK\_COMPRESS\_METHOD

Indicates what compression method shall be used to compress the included files. Valid options are gzip ,bzip2 , and 7zip . Default value is gzip .

## QDK\_COMPRESS\_FILE

Name of the compressed data archive that is included in the QPKG package. Depends on the selectedcompression method. With gzip compression the name is data.tar.gz, with bzip2 it is data.tar.bz2, andwith 7zip it is data.tar.7z.

## QDK\_CONTROL\_FILE

Name of the included archive with meta-files \\(the generic and package specific installation scripts, theQPKG configuration file, and the optional files with MD5 checksums and configuration files.\\) Defaultname is control.tar. Note that this name is only used internally by qbuild; it is not used at the installation.

## QDK\_SETUP

Location of the setup script.QDK\\_TEARDOWNLocation of the teardown script.

## QDK\_PRE\_BUILD

Location of the pre-build script.

## QDK\_POST\_BUILD

Location of the post-build script.

## QDK\_ROOT\_DIR

Location of the root directory with the subdirectories, files, and meta-data that shall be used for theQPKG package. Default value is current directory.

## QDK\_BUILD\_DIR

Location where the built QPKG package shall be placed. Default value is $QDK\_ROOT\_DIR \/build.QDK\_BUILD\_VERSIONVersion that shall be used for the built QPKG package. Default is to use the QPKG\_VER value from theQPKG configuration file, but if QDK\_BUILD\_VERSION is defined then that value is used instead and theQPKG\_VER value in the QPKG configuration file is updated with the specified version.

## QDK\_BUILD\_MODEL

Model the built QPKG package can be installed on. Default is to allow installation on any model. Thecheck for the model tag is only performed by the web installation; installing a QPKG package on thecommand line is not affected by this setting.

## QDK\_BUILD\_ARCH

QPKG packages shall be built for the specified architectures. Supported values are arm-x09, arm-x19,x86, and x86\\_64. Multiple values must be comma-separated. No default value; by default qbuilddetermines what to build based on the available files and directories in $QDK\\_ROOT\\_DIR .An architecture check is added automatically when an architecture specific package is built. If atinstallation of the QPKG package the check fails then the installation exits with a system log message,"Wrong architecture: $QPKG\\_NAME $QPKG\\_VER is built for ARCH " .

## QDK\_RSYNC\_EXCLUDE

Exclude patterns for rsync that are used to not include files matching the pattern in the data packagespecified in QDK\_COMPRESS\_FILE . The format is QDK\_RSYNC\_EXCLUDE= "--exclude=PATTERN". It ispossible to specify multiple patterns, QDK\_RSYNC\_EXCLUDE= "--exclude=PATTERN1--exclude=PATTERN2 ...".

## QDK\_RSYNC\_EXCLUDE\_FROM

Location of file with exclude patterns. Similar to QDK\\_RSYNC\\_EXCLUDE , but specifies a file with excludepatterns \\(one per line\\). The format is QDK\\_RSYNC\\_EXCLUDE\\_FROM= "--exclude-from=FILE".

## QDK\_SIGN

If set to TRUE then a digital signature is added to the QPKG package at build time. The gpg2 applicationmust be installed and in the package builder's path or the location specified in QDK\\_GPG\\_APP or the buildfails with an error.

## QDK\_GPG\_APP

Path to gpg2 application. Default is to search for it in the user's path.

## QDK\_GPG\_NAME

Identity of private key that shall be used for the digital signature.

## QDK\_GPG\_PUBKEYRING

Path to public keyring that shall be used to verify a digital signature. Default is \/etc\/config\/qpkg.gpg.

## QDK\_GPG\_KEYPATH

Path to directory with default keyrings to be used when adding signatures. Default is the $GNUPGHOMEenvironment variable, but if the keyrings are located at as location where gpg2 doesn't expect them thisvariable can be used to specify the location.

## QDK\_SIGNATURE

Use specified type of digital signature. Currently, only gpg is supported.

## QDK\_DATA\_DIR\_ICONS

Location of directory with icons for the packaged software. Default location is a directory named icons in$QDK\\_ROOT\\_DIR . The value must be a full path or a path relative to $QDK\\_ROOT\\_DIR .

The icons shall be named ${QPKG\\_NAME} .gif, ${QPKG\\_NAME} \\_80.gif, and ${QPKG\\_NAME} \\_gray.gif,

* ${QPKG\_NAME} .gif is the image displayed in the web interface when the QPKG is enabled. It shouldbe a GIF image of 64x64 pixels.
* ${QPKG\_NAME} \_gray.gif is the image displayed in the web interface when the QPKG is disabled. Itshould be a GIF image of 64x64 pixels. It is usually a greyscale version of the ${QPKG\_NAME} .gifimage, but that is not a requirement.
* ${QPKG\_NAME} \_80.gif is the image displayed in the pop-up dialog \(with information about theQPKG and the buttons to enable, disable, and remove\). It should be a GIF image of 80x80 pixels.

If no icons are included then the QPKG is given default icons at installation.

## QDK\_DATA\_DIR\_ARM\_X19

Location of directory with files specific to arm-x19 packages. Default location is a directory named arm-x19 in $QDK\_ROOT\_DIR . The value must be a full path or a path relative to $QDK\_ROOT\_DIR .

## QDK\_DATA\_DIR\_ARM\_X31

Location of directory with files specific to arm-x31 packages. Default location is a directory named arm-x31 in $QDK\_ROOT\_DIR . The value must be a full path or a path relative to $QDK\_ROOT\_DIR .

## QDK\_DATA\_DIR\_ARM\_X41

Location of directory with files specific to arm-x41 packages. Default location is a directory named arm-x41 in $QDK\_ROOT\_DIR . The value must be a full path or a path relative to $QDK\_ROOT\_DIR .

## QDK\_DATA\_DIR\_ARM\_64

Location of directory with files specific to arm\_64 packages. Default location is a directory named arm-64 in $QDK\_ROOT\_DIR . The value must be a full path or a path relative to $QDK\_ROOT\_DIR .

## QDK\_DATA\_DIR\_X86

Location of files specific to x86 packages. Default location is a directory named x86 in $QDK\_ROOT\_DIR .The value must be a full path or a path relative to $QDK\_ROOT\_DIR .

## QDK\_DATA\_DIR\_X86\_64

Location of directory with files specific to x86 \(64-bit\) packages. Default location is a directory named x86\_64 in $QDK\_ROOT\_DIR . The value must be a full path or a path relative to $QDK\_ROOT\_DIR .

## QDK\_DATA\_DIR\_X86\_CE53xx

Location of directory with files specific to x86 packages. Default location is a directory named x86\_ce53xx in $QDK\_ROOT\_DIR . The value must be a full path or a path relative to $QDK\_ROOT\_DIR .

## QDK\_DATA\_DIR\_SHARED

Location of directory with files common to all architectures. These files are included before the architecture specific files when the package is created to allow any architecture specific files to replaceshared files. Default location is a directory named shared in $QDK\_ROOT\_DIR . The value must be a fullpath or a path relative to $QDK\_ROOT\_DIR .

## QDK\_DATA\_DIR\_CONFIG

Location of directory with full path configuration files. Default location is a directory named config in$QDK\_ROOT\_DIR . The value must be a full path or a path relative to $QDK\_ROOT_DIR .The complete path under the file system root \(\/\) shall be created in this directory. For example, if aconfiguration file shall be installed in \/etc\/config\/myApp.conf then $QDKDATADIRCONFIG shouldcontain the subdirectory structure etc\/config\/ in which the myApp.conf file is placed.Note that configuration files that are located relative to the QPKG directory \( $SYSQPKGDIR \) can beplaced in any of $QDD\_DATA\_DIR\_ARM\_X19, $QDK\_DATA\_DIR\_ARM\_X31, $QDK\_DATA\_DIR\_ARM\_X41, $QDK\_DATA\_DIR\_ARM\_64, $QDK\_DATA\_DIR\_X86,$QDK\_DATA\_DIR\_X86\_64_, $QDK\_DATA\_DIR\_SHARED, or $QDK\_DATA\_DIR\_CONFIG. It is onlyexternal configuration files that must be placed in $QDK\_DATA\_DIR\_CONFIG.

## QDK\_DATA\_FILE

Name of local data package that shall be used when creating the QPKG package. If defined then$QDK\_DATA\_DIR\_ICONS , $QDK\_DATA\_DIR\_X19 , $QDK\_DATA\_DIR\_X86 ,$QDK\_DATA\_DIR\_X86\_64 , and $QDK\_DATA\_DIR\_SHARED are ignored and no other data files or iconsare included in QPKG package. If QDK\_EXTRA\_FILE has been specified then the extra data packages arestill included, though.

## QDK\_EXTRA\_FILE

Name of extra data package that shall be included in the QPKG package. The value must be a full path ora path relative to $QDK\_ROOT\_DIR . Any included extra data package must be extracted by packagespecific functions. It is possible to add multiple QDK\_EXTRA\_FILE to specify more than one extra datapackage. If Optware packages are assigned to QDK\_EXTRA\_FILE variable then an index file\(Packages.gz\) for all included Optware packages is generated automatically and attached to the QPKGpackage. This file is used at installation to create a local repository with the included Optware packagesand include it in a possible installation\/upgrade of required Optware packages.

## QDK\_QPKG\_FILE

Name of the last built QPKG package \(stored in $QDK\_BUILD\_DIR \). Can be accessed in the post-buildscript to perform any kind of actions on the package \(for example zip the file and upload it to a remotelocation, although if more than one package is created it might make sense to wait with the upload untilthe teardown script is run after all packages have been built\). $QDK\_QPKG\_FILE is reset before the pre-build script is run to allow the script to set a new value, before a default value is assigned.

