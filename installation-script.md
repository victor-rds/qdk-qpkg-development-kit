# Installation Script

The installation script is divided into two parts, one with generic installation functions common to all QPKGpackages and one with package specific functions. Only the file with package specific functions must becreated when building a QPKG package using QDK.

# Generic Installation Script

The generic script, by default a file named qinstall.sh, performs the installation or upgrade of an alreadyinstalled package in three steps, pre-install, install, and post-install.

In the pre-install phase, the base directory for the QPKG installation is located and system variables areassigned valid values, configuration files are handled, and the current status of the QPKG \(enabled ordisabled\) is stored to be able to restore it later. The specified service program is stopped and then anypackage specific pre-install code is run.

In the install phase the data package is extracted in the QPKG directory, configuration files restored, andthen any package specific install code is run.

In the post-install phase any QPKG icons are copied to the correct location, symbolic links for the serviceprogram are created, the QPKG is registered in \/etc\/config\/qpkg.conf, and then any package specific post-install code is run.

To replace the generic installation script with a different script the QDK\_INSTALL\_SCRIPT variable can beassigned the location of the script. The generic installation script includes several different system definitionsthat can be used in the package specific script \(use them as if they were read-only with the only exceptionbeing SYS\_QPKG\_SERVICE\_ENABLED , which could be changed if required\).

##### SYS\_EXTRACT\_DIR

Path to directory with extracted files from QPKG packages. The value is assigned at run-time.SYS\_HOSTNAMEHost name for system the QPKG is installed on.

##### SYS\_CONFIG\_DIR

Path to directory where configuration files are stored. Default is \/etc\/config

##### SYS\_INIT\_DIR

Path to directory where init-scripts are stored. Default is \/etc\/init.d

##### SYS\_STARTUP\_DIR

Path to directory with symbolic links to init-scripts that shall be run at system startup. Default is\/etc\/rcS.d

##### SYS\_SHUTDOWN\_DIR

Path to directory with symbolic links to init-scripts that shall be run at system shutdown. Default is\/etc\/rcK.d

##### SYS\_RSS\_IMG\_DIR

Path to directory with icons for the web interface. Default is \/home\/httpd\/RSS\/imagesSYS\_QPKG\_BASEBase location. Always assigned the location of the volume with the Public share, for example,\/share\/MD0\_DATA.

##### SYS\_QPKG\_INSTALL\_PATH

Base location of QPKG installed packages. Same as $SYS\_QPKG\_BASE \/.qpkg

##### SYS\_QPKG\_DIR

Location of installed software. Same as $SYS\_QPKG\_INSTALL\_PATH\/$QPKG\_NAME

##### SYS\_QPKG\_DATA\_FILE\_GZIP

Location of gzip compressed tar package with the data files. Default .\/data.tar.gz

##### SYS\_QPKG\_DATA\_FILE\_BZIP2

Location of bzip2 compressed tar package with the data files. Default .\/data.tar.bz2

##### SYS\_QPKG\_DATA\_FILE\_7ZIP

Location of 7-zip compressed tar package with the data files. Default .\/data.tar.7z

##### SYS\_QPKG\_DATA\_FILE

Location of the tar package with the data files. Assigned the value of eitherSYS\_QPKG\_DATA\_FILE\_GZIP, SYS\_QPKG\_DATA\_FILE\_BZIP2, or SYS\_QPKG\_DATA\_FILE\_7ZIPat runtime depending on what type of data archive that is included in the QPKG package.

##### SYS\_QPKG\_DATA\_MD5SUM\_FILE

Location of the optional file with the md5sum values. Default .\/md5sum

##### SYS\_QPKG\_DATA\_CONFIG\_FILE

Location of the optional file with configuration files. Default .\/conf.tar.gz

##### SYS\_QPKG\_CONFIG\_FILE

Location of system-wide configuration file for all installed QPKG packages. Default value is$SYS\_CONFIG\_DIR \/qpkg.conf. The following field names can be used when accessing the configurationfile.

SYS\_QPKG\_CONF\_FIELD\_QPKGFILE, SYS\_QPKG\_CONF\_FIELD\_NAME,SYS\_QPKG\_CONF\_FIELD\_VERSION, SYS\_QPKG\_CONF\_FIELD\_ENABLE,SYS\_QPKG\_CONF\_FIELD\_DATE, SYS\_QPKG\_CONF\_FIELD\_SHELL,SYS\_QPKG\_CONF\_FIELD\_INSTALL\_PATH, SYS\_QPKG\_CONF\_FIELD\_CONFIG\_PATH,SYS\_QPKG\_CONF\_FIELD\_WEBUI, SYS\_QPKG\_CONF\_FIELD\_WEBPORT,SYS\_QPKG\_CONF\_FIELD\_SERVICEPORT, SYS\_QPKG\_CONF\_FIELD\_SERVICE\_PIDFILE , andSYS\_QPKG\_CONF\_FIELD\_AUTHOR

##### SYS\_QPKG\_SERVICE\_ENABLED

Used to determine if the QPKG should be enabled or disabled after the installation\/upgrade is finished. Atan installation the value is always set to FALSE , while at an upgrade the current status of the QPKG isassigned to this variable before any of the package specific functions are executed. If set to TRUE then theservice program is also started at the end of the installation\/upgrade \(if it fails to start then the status is setto disabled\).

##### SYS\_PUBLIC\_SHARE

Name of public system share.

##### SYS\_PUBLIC\_PATH

Location of public system share.

##### SYS\_DOWNLOAD\_SHARE

Name of system share for downloads.

##### SYS\_DOWNLOAD\_PATH

Location of system share for downloads.

##### SYS\_MULTIMEDIA\_SHARE

Name of system share for multimedia content.

##### SYS\_MULTIMEDIA\_PATH

Location of system share for multimedia content.

##### SYS\_RECORDINGS\_SHARE

Name of system share for recordings.

##### SYS\_RECORDINGS\_PATH

Location of system share for recordings.

##### SYS\_USB\_SHARE

Name of system share for USB content.SYS\_USB\_PATHLocation of system share for USB content.SYS\_WEB\_SHAREName of system share for web content.SYS\_WEB\_PATHLocation of system share for web content.Pre-defined command definitions.CMD\_AWK, CMD\_CAT, CMD\_CHMOD, CMD\_CHOWN, CMD\_CP, CMD\_CUT, CMD\_DATE, CMD\_ECHO,CMD\_EXPR, CMD\_FIND, CMD\_GETCFG, CMD\_GREP, CMD\_GZIP, CMD\_HOSTNAME, CMD\_LN,CMD\_LOG\_TOOL, CMD\_MD5SUM, CMD\_MKDIR, CMD\_MV, CMD\_PKG\_TOOL, CMD\_RM, CMD\_RMDIR,CMD\_SED, CMD\_SETCFG, CMD\_SLEEP, CMD\_SORT, CMD\_SYNC, CMD\_TAR, CMD\_TOUCH,CMD\_WGET, CMD\_WLOG, CMD\_XARGS, and CMD\_7Z

CMD\_PKG\_TOOL is assigned the path to either Optware's ipkg tool or opkg's opkg tool at runtime \(if found\).

# Package Specific Installation Functions

The package specific functions \(by default included in a file named package\_routines\) include any extraactions that shall be performed at installation. All the variables defined in the QPKG configuration file and inthe generic installation script can be accessed in the package specific functions.

The following support functions can be used in the package specific functions.

##### log "MSG"

Outputs MSG to terminal \(stdout\) and system log.

##### warn\_log "MSG"

Outputs MSG to terminal \(stderr\) and system log.

##### err\_log "MSG"

Outputs MSG to terminal \\(stderr\\) and system log. Also alerts the web interface about the failure andterminates the installation\\/upgrade.

The error message always includes " $QPKG\\_NAME $QPKG\\_VER installation failed." followed by themessage given to err\\_log . For example with a QPKG named myApp, version 0.1:

   \`err\_log "Data file not found."\`

would result in this message

  \`myApp 0.1 installation failed. Data file not found.\`

##### get\_share\_path "SHARE NAME" VARIABLE

Assign location of given share to variable. For example,

`get_share_path openssh OPENSSH_PATH`

would assign the location on the HDD for \/share\/openssh to a variable named OPENSSH\_PATH.

##### add\_qpkg\_config "CONFIGURATION FILE" MD5SUM

Add configuration file with given md5sum value . The md5sum value is for the original file \(i.e. the oneincluded in the package\). For configuration files created at installation time the md5sum should be set to0. The function checks that the configuration file isn't already added to $SYS\_QPKG\_CONFIG\_FILEbefore adding it, so it won't modify existing values.

This function could be used to make existing configuration files that were not previously specified withQPKG\_CONFIG to be handled as if they actually were specified. See Creating a Simple QPKG Package foran example.

##### set\_qpkg\_config "CONFIGURATION FILE" MD5SUM

Update existing configuration file with given md5sum value .

##### extract\_data ARCHIVE \[DIRECTORY\]

Extract specified tar archive to given directory. If the directory is not specified then the default is to use$SYS\_QPKG\_DIR . Can be used to extract any extra data archives that have been included in the QPKG package.

If a version check shall be added in one of the package specific functions then the following support functioncould be used to compare the versions. They all take two version strings as input and return 0 if the test issuccessful, otherwise they return 1.

is\_equal, is\_unequal, is\_less\_or\_equal, is\_less, is\_greater,is\_greater\_or\_equal

The package\_routines file has the following content by default.

`#######################################################################`

`#List of available definitions (it's not necessary to uncomment them)`

`#######################################################################`

`##### Command definitions #####`

`#CMD_AWK="/bin/awk":`

`#SYS_WEB_PATH=""`

`######################################################################`

`# All package specific functions shall call 'err_log MSG' if an error`

`# is detected that shall terminate the installation.`

`######################################################################`

`#`

`######################################################################`

`# Define any package specific operations that shall be performed when`

`# the package is removed.`

`######################################################################`

`#PKG_PRE_REMOVE="{`

`#}"`

`#`

`#PKG_MAIN_REMOVE="{`

`#}"`

`#`

`#PKG_POST_REMOVE="{`

`#}"`

`#`

`######################################################################`

`# Define any package specific initialization that shall be performed`

`# before the package is installed.`

`######################################################################`

`#pkg_init(){`

`#}`

`#`

`######################################################################`

`# Define any package specific requirement checks that shall be`

`# performed before the package is installed.`

`######################################################################`

`#pkg_check_requirement(){`

`#}`

`#`

`######################################################################`

`# Define any package specific operations that shall be performed when`

`# the package is installed.`

`######################################################################`

`#pkg_pre_install(){`

`#}`

`#`

`#pkg_install(){`

`#}`

`#`

`#pkg_post_install(){`

`#}`

The package specific functions shall only return when the function has been successful or any detected errorcan be ignored, because the generic installation script ignores any returned values from the package specificfunctions. If an error is detected that should terminate the installation or upgrade then err\_log shall becalled with an error message.

PKG\_PRE\_REMOVE , PKG\_MAIN\_REMOVE , and PKG\_POST\_REMOVE are pseudo-functions that are includedin the generated uninstall script. They can include almost the same code as a normal function, but variablesdefined in the functions must be escaped to be included in the uninstall script. For example, $VAR must beincluded as $VAR or the variable is replaced with its value already when the uninstall script is created\(which most likely would result in an empty value\). Also, any command substitutions, $\(command\) \(or theolder format \`command\`\) must be escaped if the command should be executed at runtime instead of whenthe uninstall script is created.

