# Control the Build

When building a QPKG package it is possible to change some global settings. To change the location of thefiles and meta-data that shall be used for the QPKG package the --root option can be used \(default location isto use the files in the current directory, '.'\). The version is by default set to the same as the value of theQPKG\_VER variable in the QPKG configuration file, but it can also be specified on the command line usingthe --build-version option, which also updates QPKG\_VER in the QPKG configuration file.

When qbuild is run it determines what architectures that QPKG packages shall be created for by looking atwhat data directories that are not empty. It is possible to override this and specify what architectures thatshall be included by using the --build-arch option \(supported values: arm-x09, arm-x19, x86, and x86\_64\).Only one architecture can be specified per option, but it is possible to repeat the option on the command lineto add multiple architectures.

When configuration files have been specified using QPKG\_CONFIG then qbuild checks that the specified filesexists or exits with an error. When the specified configuration file is created at installation then the --force-config option can be used to ignore that the file is missing from the package itself and still handle the filecorrectly at an upgrade.

The result is by default placed in a directory named build relative to $QDK\_ROOT\_DIR , but the --build-diroption can be used to specify a different directory for the result. If a full path is not specified then thespecified location is relative to $QDK\_ROOT\_DIR .

Using the --build-model option it is possible to add an extra check for a given model tag at installation. If thegiven tag doesn't match the model for the device the QPKG package is installed on then the web interfacefails to install the package.

It is possible to specify the compression methods using --gzip, --bzip2, or --7zip. By default the files arecompressed using gzip.

