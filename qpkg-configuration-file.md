# QPKG Configuration File

We begin with a description of the QPKG configuration file, which is required when building a QPKGpackage. The QPKG configuration file \(by default a file named qpkg.cfg\) defines common variables for theQPKG package. Most of the variables are only used at installation, but some of them are also used when theQPKG package is built.

This is an example of a configuration file,

`QPKG_NAME="QDK"QPKG_VER="2.14"`

`QPKG_AUTHOR="micke"`

`QPKG_LICENSE="GPLv2+"`

`QPKG_SUMMARY="QDK (QPKG Development Kit) is used to create QPKG packages."`

`QPKG_SERVICE_PROGRAM="qdk"QPKG_RC_NUM="101"`

The first three variables, QPKG\_NAME , QPKG\_VER , and QPKG\_AUTHOR , must always be included.

##### QPKG\_NAME

Name of the software being packaged. Usually, the name used for a package would be identical to thesoftware being packaged. No white-space is allowed in the name or QDK will only use the first part ofthe name \(up to the first space\). The name must be 20 characters or less.

##### QPKG\_DISPLAY\_NAME

The QPKG name displayed on QTS Web UI

##### QPKG\_VER

Version of the software being packaged. This should be as close as possible to the format of the originalsoftware's version. No white-space is allowed in the version. If it does, QDK will only use the first part ofthe version. It is recommended to only use alphanumeric characters, periods, and dashes. The value mustbe 10 characters or less.

##### QPKG\_AUTHOR

Author or maintainer of the package \(not necessarily the author of the software that is being packaged,though.\)

The following variables are optional, but most QPKG packages would at least include QPKG\_RC\_NUM and QPKG\_SERVICE\_PROGRAM.

##### QPKG\_RC\_NUM

Preferred number in start\/stop sequence. Note that the value is currently ignored by the QNAP NAS aftera reboot at which point the actual number is assigned depending on the order of the applications in\/etc\/config\/qpkg.conf starting with 100 for the first application.

##### QPKG\_REQUIRE

This field is used to make sure that any packages required for the current package to operate are installed.The format is a package name followed by an optional version comparison, =, !=, &lt;, &gt;, &lt;=, or &gt;=, and aversion specification. It is possible to use \| to separate requirements where only one of them has to betrue. The package name is either another QPKG or it could be an Optware package which is indicated byadding an OPT\/ prefix to the package name. Optware packages are installed automatically if missing \(orhave incorrect version\). If more than one requirement shall be included they must be separated bycomma. If the specified requirement is not fulfilled then the installation is terminated with an errormessage to the system log.Example, QPKG\_REQUIRE="Python &gt;= 2.7, Optware \| opkg, OPT\/openssh"

##### QPKG\_CONFLICT

Logical complement to the QPKG\_REQUIRE field. The QPKG\_CONFLICT specifies what packages cannot5be installed if the current package is to operate properly. This field has the same format as theQPKG\_REQUIRE field, namely, a package name optionally followed by a version comparison. It is notpossible to use \| to separate statements \(neither is it logical to use it for QPKG\_CONFLICT .\)Example, QPKG\_CONFLICT="SSOTS"

##### QPKG\_LICENSE

License for the packaged application. For example, GPLv2+ , GPLv3+ , BSD , or Commercial.

##### QPKG\_SUMMARY

One-line description of the packaged application.

##### QPKG\_CONFIG

Can be added to specify a configuration file that shall receive special handling when the package isupgraded. The value can be be a path relative to $SYS\_QPKG\_DIR or a full path.

When the installed configuration file has never been modified then it is replaced with the new file. Whenthe installed file has been modified and the new and original files are identical it is assumed that theinstalled file is still valid and it is left unchanged. When the installed file has been modified, but the newand original files are not identical it is possible that the local modifications are not valid any longer, so theinstalled file is replaced with the new file. A backup of the installed file is, however, saved with the namefile .qdksave and a message is written to the system log .

The last scenario is when the configuration file was not installed by a package with support for keepingtrack of configuration files, so it is not possible to determine whether the current file is OK or not. In thiscase the installed file is replaced with the one from the package \(which is known to work\), a backup ofthe installed file is saved with the name file .qdkorig, and a message is written to the system log.

Configuration files that are automatically generated at installation time can also be specified usingQPKG\_CONFIG , but in that case it is necessary to use the --force-config option or setQDK\_FORCE\_CONFIG to TRUE or the build fails because of missing configuration files.

##### QPKG\_SERVICE\_PROGRAM

Init-script used to control the start and stop of the installed software. This script shall support start, stop,and restart commands. Any other command is optional. It is not necessary to include an init-script, but itwould seriously limit the use of most QPKG packages to not include one.

##### QPKG\_SERVICE\_PORT

Port number used by the installed application's service program.

##### QPKG\_SERVICE\_PIDFILE

Location where the PID of a running service shall be stored.

##### QPKG\_WEBUI

Relative path to installed application's web interface \(the specified path is relative the configured locationof web server data; usually \/share\/Web or \/share\/Qweb.\) The specified path must start with a '\/'. Thedisplayed link can only be accessed when the QPKG is enabled. A default value of '\/' is set automaticallyat installation if QPKG\_WEB\_PORT has been given a value and QPKG\_WEBUI is empty.

##### QPKG\_WEB\_PORT

Port number for the web interface. If empty and QPKG\_WEBUI is defined then the default is to use thesame port number as the web server \(usually port 80\).

##### QPKG\_WEB\_SSL\_PORT

SSL Port number for the HTTPS interface. If empty and QPKG\_WEBUI is defined then the default is to use thesame port number as the web server \(usually port 8081\).

##### QPKG\_USE\_PROXY

Optional. 1 means use QTS HTTP Proxy. When the QPKG has its own HTTP service port, but still want clients to connect to QTS via QTS HTTP port. QTS provides the proxy function for client to connect to the QPKG web UI via QTS HTTP port (default 8080).

##### QPKG\_PROXY\_PATH

Optional. If the attribute/ value is set, the value will be employed as the proxy path. 

##### QPKG\_DESKTOP\_APP

Optional. 1 means to open the QPKG’s Web UI on QTS desktop.

##### QTS\_MINI\_VERSION

QPKG can only run on firmware version Mini or newer.

##### QTS\_MAX\_VERSION

QPKG can only run on firmware version Max or older.

##### QPKG\_VOLUME\_SELECT

Optional.
0. Disable application routing rule.
1. Allow outgoing data packet apply to routing rule.
2. Allow incoming data packet bind to network interface. (not ready)
3. 1 + 2 (not ready).

##### QPKG\_TIMEOUT

Optional since QTS 4.1.0. Timeout seconds of starting/ stopping the QPKG service. When start the QPKG, it will wait for starting this QPKG according the first timeout seconds (the first integer). When stop or shutdown the system, the second integer is taken as the timeout seconds.

##### QPKG\_VISIBLE

Optional. If the QPKG has web UI, show this QPKG on the Main menu of 1(default): administrators, 2: all NAS users.

##### QPKG\_SERVICE\_PROGRAM\_CHROOT

Init-script that controls the start and stop of the installed software when running in the chrootenvironment. This script shall support start, stop, and restart commands. Any other command is optional.No default.The following QDK specific variables can also be included in the configuration file to be used when theQPKG package is built. They are described in the QDK Variables chapter.

QDK\_DATA\_DIR\_ICONS, QDK\_DATA\_DIR\_X19, QDK\_DATA\_DIR\_X31 ,QDK\_DATA\_DIR\_X41, QDK\_DATA\_DIR\_X86,QDK\_DATA\_DIR\_X86\_64, QDK\_DATA\_DIR\_SHARED, QDK\_DATA\_DIR\_CONFIG, QDK\_DATA\_FILE,QDK\_EXTRA\_FILE

The qbuild application performs a simple sanity check of the QPKG configuration file when the buildprocess is started. If the file is found to be corrupt then the build process exits. Also, qbuild will refuse tobuild a QPKG package unless QPKG\_AUTHOR , QPKG\_NAME , and QPKG\_VER have been assigned values. IfQPKG\_SERVICE\_PROGRAM is undefined then a warning will be issued, but the build will continue. If QPKG\_NAME is found to be more than 20 characters or QPKG\_VER more than 10 characters then the values aretruncated with a warning. Any space in the name or version also results in a warning and truncation \(only thestring up to the first space is used\).

