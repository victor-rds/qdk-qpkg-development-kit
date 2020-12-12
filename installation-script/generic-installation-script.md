# Generic Installation Script

The generic script, by default a file named **qinstall.sh**, performs the installation or upgrade of an already installed package in three steps, pre-install, install, and post-install.

In the pre-install phase, the base directory for the QPKG installation is located and system variables are assigned valid values, configuration files are handled, and the current status of the QPKG \(enabled or disabled\) is stored to be able to restore it later. The specified service program is stopped and then any package specific pre-install code is run.

In the install phase the data package is extracted in the QPKG directory, configuration files restored, and then any package specific install code is run.

In the post-install phase any QPKG icons are copied to the correct location, symbolic links for the service program are created, the QPKG is registered in `/etc/config/qpkg.conf`, and then any package specific post-install code is run.

To replace the generic installation script with a different script the **QDK\_INSTALL\_SCRIPT** variable can be assigned the location of the script. The generic installation script includes several different system definitions that can be used in the package specific script \(use them as if they were read-only with the only exception being **SYS\_QPKG\_SERVICE\_ENABLED**, which could be changed if required\).

## SYS\_EXTRACT\_DIR

Path to directory with extracted files from QPKG packages. The value is assigned at run-time. **SYS\_HOSTNAME** Host name for system the QPKG is installed on.

## SYS\_CONFIG\_DIR

Path to directory where configuration files are stored. Default is `/etc/config`

## SYS\_INIT\_DIR

Path to directory where init-scripts are stored. Default is `/etc/init.d`

## SYS\_STARTUP\_DIR

Path to directory with symbolic links to init-scripts that shall be run at system startup. Default is `/etc/rcS.d`

## SYS\_SHUTDOWN\_DIR

Path to directory with symbolic links to init-scripts that shall be run at system shutdown. Default is `/etc/rcK.d`

## SYS\_RSS\_IMG\_DIR

Path to directory with icons for the web interface. Default is `/home/httpd/RSS/images` 

## SYS\_QPKG\_BASE

Base location.  Always assigned the location of the volume with the Public share, for example: `/share/MD0_DATA`.

## SYS\_QPKG\_INSTALL\_PATH

Base location of QPKG installed packages. Same as `$SYS_QPKG_BASE/.qpkg`

## SYS\_QPKG\_DIR

Location of installed software. Same as `$SYS_QPKG_INSTALL_PATH/$QPKG_NAME`

## SYS\_QPKG\_DATA\_FILE\_GZIP

Location of gzip compressed tar package with the data files. Default `./data.tar.gz`

## SYS\_QPKG\_DATA\_FILE\_BZIP2

Location of bzip2 compressed tar package with the data files. Default `./data.tar.bz2`

## SYS\_QPKG\_DATA\_FILE\_7ZIP

Location of 7-zip compressed tar package with the data files. Default `./data.tar.7z`

## SYS\_QPKG\_DATA\_FILE

Location of the tar package with the data files. Assigned the value of either **SYS\_QPKG\_DATA\_FILE\_GZIP**, **SYS\_QPKG\_DATA\_FILE\_BZIP2**, or **SYS\_QPKG\_DATA\_FILE\_7ZIP** at runtime depending on what type of data archive that is included in the QPKG package.

## SYS\_QPKG\_DATA\_MD5SUM\_FILE

Location of the optional file with the md5sum values. Default `./md5sum`

## SYS\_QPKG\_DATA\_CONFIG\_FILE

Location of the optional file with configuration files. Default `./conf.tar.gz`

## SYS\_QPKG\_CONFIG\_FILE

Location of system-wide configuration file for all installed QPKG packages. Default value is `$SYS_CONFIG_DIR/qpkg.conf`. The following field names can be used when accessing the configuration file.

* SYS\_QPKG\_CONF\_FIELD\_QPKGFILE
* SYS\_QPKG\_CONF\_FIELD\_NAME
* SYS\_QPKG\_CONF\_FIELD\_VERSION
* SYS\_QPKG\_CONF\_FIELD\_ENABLE
* SYS\_QPKG\_CONF\_FIELD\_DATE
* SYS\_QPKG\_CONF\_FIELD\_SHELL
* SYS\_QPKG\_CONF\_FIELD\_INSTALL\_PATH
* SYS\_QPKG\_CONF\_FIELD\_CONFIG\_PATH
* SYS\_QPKG\_CONF\_FIELD\_WEBUI
* SYS\_QPKG\_CONF\_FIELD\_WEBPORT
* SYS\_QPKG\_CONF\_FIELD\_SERVICEPORT
* SYS\_QPKG\_CONF\_FIELD\_SERVICE\_PIDFILE
* SYS\_QPKG\_CONF\_FIELD\_AUTHOR
* SYS\_QPKG\_CONF\_FIELD\_RC\_NUMBER

## SYS\_QPKG\_SERVICE\_ENABLED

Used to determine if the QPKG should be enabled or disabled after the installation/upgrade is finished. At an installation the value is always set to FALSE, while at an upgrade the current status of the QPKG is assigned to this variable before any of the package specific functions are executed. If set to TRUE then the service program is also started at the end of the installation/upgrade \(if it fails to start then the status is set to disabled\).

## SYS\_PUBLIC\_SHARE

Name of public system share.

## SYS\_PUBLIC\_PATH

Location of public system share.

## SYS\_DOWNLOAD\_SHARE

Name of system share for downloads.

## SYS\_DOWNLOAD\_PATH

Location of system share for downloads.

## SYS\_MULTIMEDIA\_SHARE

Name of system share for multimedia content.

## SYS\_MULTIMEDIA\_PATH

Location of system share for multimedia content.

## SYS\_RECORDINGS\_SHARE

Name of system share for recordings.

## SYS\_RECORDINGS\_PATH

Location of system share for recordings.

## SYS\_USB\_SHARE

Name of system share for USB content.

## SYS\_USB\_PATH

Location of system share for USB content.

## SYS\_WEB\_SHARE

Name of system share for web content.

## SYS\_WEB\_PATH

Location of system share for web content.

## CMD\_PKG\_TOOL

Is assigned the path to either Optware's ipkg tool or opkg's opkg tool at runtime \(if found\).

## Command Definitions.



* **CMD\_AWK** \(`/bin/awk`\)
* **CMD\_CAT** \(`/bin/cat`\)
* **CMD\_CHMOD** \(`/bin/chmod`\)
* **CMD\_CHOWN** \(`/bin/chown`\)
* **CMD\_CP** \(`/bin/cp`\)
* **CMD\_CUT** \(`/bin/cut`\)
* **CMD\_DATE** \(`/bin/date`\)
* **CMD\_ECHO** \(`/bin/echo`\)
* **CMD\_EXPR** \(`/usr/bin/expr`\)
* **CMD\_FIND** \(`/usr/bin/find`\)
* **CMD\_GETCFG** \(`/sbin/getcfg`\)
* **CMD\_GREP** \(`/bin/grep`\)
* **CMD\_GZIP** \(`/bin/gzip`\)
* **CMD\_HOSTNAME** \(`/bin/hostname`\)
* **CMD\_LN** \(`/bin/ln`\)
* **CMD\_LOG\_TOOL** \(`/sbin/log_tool`\)
* **CMD\_MD5SUM** \(`/bin/md5sum`\)
* **CMD\_MKDIR** \(`/bin/mkdir`\)
* **CMD\_MV** \(`/bin/mv`\)
* **CMD\_RM** \(`/bin/rm`\)
* **CMD\_RMDIR** \(`/bin/rmdir`\)
* **CMD\_SED** \(`/bin/sed`\)
* **CMD\_SETCFG** \(`/sbin/setcfg`\)
* **CMD\_SLEEP** \(`/bin/sleep`\)
* **CMD\_SORT** \(`/usr/bin/sort`\)
* **CMD\_SYNC** \(`/bin/sync`\)
* **CMD\_TAR** \(`/bin/tar`\)
* **CMD\_TOUCH** \(`/bin/touch`\)
* **CMD\_WGET** \(`/usr/bin/wget`\)
* **CMD\_WLOG** \(`/sbin/write_log`\)
* **CMD\_XARGS** \(`/usr/bin/xargs`\)
* **CMD\_7Z** \(`/usr/local/sbin/7z`\)

