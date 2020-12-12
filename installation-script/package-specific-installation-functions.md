# Package Specific Installation Functions

The package specific functions \(by default included in a file named package\_routines\) include any extra actions that shall be performed at installation. All the variables defined in the QPKG configuration file and in the generic installation script can be accessed in the package specific functions.

The following support functions can be used in the package specific functions.

## log "MSG"

Outputs MSG to terminal \(stdout\) and system log.

## warn\_log "MSG"

Outputs MSG to terminal \(stderr\) and system log.

## err\_log "MSG"

Outputs MSG to terminal \(stderr\) and system log. Also alerts the web interface about the failure and terminates the installation/upgrade.

The error message always includes `"$QPKG_NAME $QPKG_VER installation failed."` followed by the message given to err\_log . 

For example with a QPKG named myApp, version 0.1: `err_log "Data file not found."` would result in this message `myApp 0.1 installation failed. Data file not found.`

## get\_share\_path "SHARE NAME" VARIABLE

Assign location of given share to variable. For example: `get_share_path openssh OPENSSH_PATH` would assign the location on the HDD for `/share/openssh` to a variable named **OPENSSH\_PATH**.

## add\_qpkg\_config "CONFIGURATION FILE" MD5SUM

Add configuration file with given md5sum value . The md5sum value is for the original file \(i.e. the one included in the package\). For configuration files created at installation time the md5sum should be set to 0. The function checks that the configuration file isn't already added to $SYS\_QPKG\_CONFIG\_FILE before adding it, so it won't modify existing values.

This function could be used to make existing configuration files that were not previously specified with **QPKG\_CONFIG** to be handled as if they actually were specified. See Creating a Simple QPKG Package for an example.

## set\_qpkg\_config "CONFIGURATION FILE" MD5SUM

Update existing configuration file with given md5sum value .

## extract\_data ARCHIVE \[DIRECTORY\]

Extract specified tar archive to given directory. If the directory is not specified then the default is to use $SYS\_QPKG\_DIR . Can be used to extract any extra data archives that have been included in the QPKG package.

If a version check shall be added in one of the package specific functions then the following support function could be used to compare the versions. They all take two version strings as input and return 0 if the test is successful, otherwise they return 1.

is\_equal, is\_unequal, is\_less\_or\_equal, is\_less, is\_greater,is\_greater\_or\_equal

The package\_routines file has the following content by default.

```bash
#######################################################################
#List of available definitions (it's not necessary to uncomment them)
#######################################################################
##### Command definitions #####
#CMD_AWK="/bin/awk":
#SYS_WEB_PATH=""
######################################################################
# All package specific functions shall call 'err_log MSG' if an error
# is detected that shall terminate the installation.
######################################################################
#
######################################################################
# Define any package specific operations that shall be performed when
# the package is removed.
######################################################################
#PKG_PRE_REMOVE="{
#}"
#
#PKG_MAIN_REMOVE="{
#}"
#
#PKG_POST_REMOVE="{
#}"
#
######################################################################
# Define any package specific initialization that shall be performed
# before the package is installed.
######################################################################
#pkg_init(){
#}
#
######################################################################
# Define any package specific requirement checks that shall be
# performed before the package is installed.
######################################################################
#pkg_check_requirement(){
#}
#
######################################################################
# Define any package specific operations that shall be performed when
# the package is installed.
######################################################################
#pkg_pre_install(){
#}
#
#pkg_install(){
#}
#
#pkg_post_install(){
#}
```

The package specific functions shall only return when the function has been successful or any detected error can be ignored, because the generic installation script ignores any returned values from the package specific functions. If an error is detected that should terminate the installation or upgrade then err\_log shall be called with an error message.

**PKG\_PRE\_REMOVE**, **PKG\_MAIN\_REMOVE**, and **PKG\_POST\_REMOVE** are pseudo-functions that are included in the generated uninstall script. They can include almost the same code as a normal function, but variables defined in the functions must be escaped to be included in the uninstall script. 

For example: `$VAR` must be included as `\$VAR` or the variable is replaced with its value already when the uninstall script is created \(which most likely would result in an empty value\). Also, any command substitutions, `$(command)` or ```command``` must be escaped if the command should be executed at runtime instead of when the uninstall script is created.

