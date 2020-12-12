# Invoking qbuild

As the previous sections have shown, we can specify most options for each QPKG package in theconfiguration file and many times this is enough. But additionally, the qbuild application provides someglobal control through powerful command-line options and also a possibility to use different configurationsthat have been set up in the user configuration file.

The qbuild application has the following options

usage:

`qbuild [--extract QPKG [DIR]] [--create-env NAME] [-s|--section SECTION]`

`[--root ROOT_DIR] [--build-arch ARCH] [--build-version VERSION]`

`[--build-number NUMBER] [--build-model MODEL] [--build-dir BUILD_DIR] [--force-config]`

`[--setup SCRIPT] [--teardown SCRIPT] [--pre-build SCRIPT]`

`[--post-build SCRIPT] [--exclude PATTERN] [--exclude-from FILE]`

`[--gzip|--bzip2|--7zip] [--sign] [--gpg-name ID] [--verify QPKG]`

`[--add-sign QPKG] [--import-key KEY] [--remove-key ID] [--list-keys]`

`[--query OPTION QPKG] [-v|--verbose] [-q|--quiet] [--strict]`

`[-?|-h|--help] [--usage] [-V|--version]`

