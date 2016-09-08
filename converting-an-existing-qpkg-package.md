# Converting an Existing QPKG Package

In the last example we convert an existing QPKG package to use QDK. The QPKG package chosen for this

task is Optware.

We create a new build environment and extracts the contents of the existing QPKG package to the shared

directory.

`    # qbuild --create-env Optware`

`    # qbuild --extract Optware_0.99.163_arm-x19.qpkg Optware/shared`

`    qinstall.sh`

`    Optware.tgz`

`    qpkg.cfg`

`    # cd Optware/shared`

`    # tar xvf Optware.tgz`

`   ./`

`    ./Optware.sh`

`    ./.qpkg_icon_80.gif`

`    ./.qpkg_icon.gif`

`    ./.qpkg_icon_gray.gif`

The directory structure should now look like this

`arm-x09/`

`arm-x19/`

`config/`

`icons/`

`shared/`

`          .qpkg_icon_80.gif`

`          .qpkg_icon.gif`

`          .qpkg_icon_gray.gif`

`          Optware.sh`

`          Optware.tgz`

`          qinstall.sh`

`          qpkg.cfg`

`x86/`

`x86_64/`

`package_routines`

`qpkg.cfg`

The Optware.tgzfile is removed and the icons are renamed to Optware.gif, Optware\_80.gif, and

Optware\_gray.gifand moved to the iconsdirectory.

`# rm Optware.tgz`

`# mv .qpkg_icon.gif ../icons/Optware.gif`

`# mv .qpkg_icon_80.gif ../icons/Optware_80.gif`

`# mv .qpkg_icon_gray.gif ../icons/Optware_gray.gif`

We edit the template qpkg.cfgusing the information we find in the qpkg.cfgfile included in the package.

Apparently, it is not possible to upgrade Optware, so any existing installation of Optware is considered a

conflict that should terminate the upgrade.

`qpkg.cfg`

`QPKG_NAME="Optware"`

`QPKG_VER="0.99.163"`

`QPKG_AUTHOR="QNAP Systems, Inc."`

`QPKG_SERVICE_PROGRAM="Optware.sh"`

`QPKG_RC_NUM="103"`

`QPKG_CONFLICT="Optware"`

After the file has been updated we can remove the old qpkg.cfgfile. We fix the path bug in the init-script,

Optware.sh, and also replace the partially buggy handling to find the location of the Optware installation

with a solution that checks for the location in \/etc\/config\/qpkg.conf.

`#!/bin/sh`

`CONF=/etc/config/qpkg.conf`

`QPKG_NAME="Optware"`

`_exit()`

`{`

`        /bin/echo -e "Error: $*"`

`        /bin/echo`

`        exit 1`

`}`

`case "$1" in`

`start)`

`ENABLED=$(/sbin/getcfg $QPKG_NAME Enable -u -d FALSE -f $CONF)`

`if [ "$ENABLED" != "TRUE" ]; then`

`         _exit "$QPKG_NAME is disabled."`

`fi`

`OPTWARE_DIR=$(/sbin/getcfg $QPKG_NAME Install_Path -d "" -f $CONF)`

`if [ -d "$OPTWARE_DIR" ]; then`

`         /bin/echo "Enable Optware/ipkg"`

`         /bin/rm -f /opt`

`         /bin/ln -s $OPTWARE_DIR /opt`

`# adding Ipkg apps into system path ...`

`       /bin/grep "PATH=.*/opt/bin.*" /etc/profile >/dev/null 1>&2 || /bin/echo ➥`

`       "export PATH=\$PATH":/opt/bin:/opt/sbin >> /etc/profile`

`else`

`        _exit "$OPTWARE_DIR: no such directory"`

`fi`

`;;`

`stop)`

`         /bin/echo "Disable Optware/ipkg"`

`         exportPATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin`

`         /bin/sync`

`         /bin/sleep 1`

`;;`

`restart)`

`        $0 stop`

`        $0 start`

`;;`

`*)`

`        echo "Usage: $0 {start|stop|restart}"`

`        exit 1`

`esac`

`exit 0`

Working with the qinstall.shscript from the Optware QPKG package we move any package specific code to

package\_routines. We also fix any bugs when moving the code, for example the bug that changes any

existing NTP settings at installation \(we change the default interval from every hour to once a day to put less

stress on the time servers, though.\) The main difference between the Optware packages for the supported

platforms is the feed setting, so we add a pre-build script that updates the feed for each package. To make it

even simpler we create a setup script that generates the pre-build script and also a teardown script to clean up

after the build process. With the settings in ~\/.qdkrcfrom the previous example the setup script is run

automatically when qbuildis run.

setup.sh

`#!/bin/sh`

`QDK_BUILD_ARCH="arm-x09,arm-x19,x86"`

`QDK_PRE_BUILD=config.sh`

`QDK_TEARDOWN=cleanup.sh`

`/bin/cat >$QDK_PRE_BUILD <<EOF`

`#!/bin/sh`

`case "\$1" in`

`arm-x09)`

`/bin/sed -i -e 's/TYPE=.*/TYPE="cs05q1armel"/' \`

`-e 's/KMOD_TYPE=.*/KMOD_TYPE="tsx09"/' $QDK_PACKAGE_ROUTINES`

`;;`

`arm-x19)`

`/bin/sed -i -e 's/TYPE=.*/TYPE="cs08q1armel"/' \`

`-e 's/KMOD_TYPE=.*/KMOD_TYPE="tsx19"/' $QDK_PACKAGE_ROUTINES`

`;;`

`x86)`

`/bin/sed -i -e 's/TYPE=.*/TYPE="ts509"/' \`

`-e 's/KMOD_TYPE=.*/KMOD_TYPE=/' $QDK_PACKAGE_ROUTINES`

`;;`

`esac`

`EOF`

`/bin/cat >$QDK_TEARDOWN <<EOF`

`#!/bin/sh`

`/bin/rm $QDK_PRE_BUILD`

`/bin/rm $QDK_TEARDOWN`

`EOF`

`package_routines`

`TYPE=`

`KMOD_TYPE=`

`FEED=http://ipkg.nslu2-linux.org/feeds/optware/${TYPE}/cross/unstable`

`if [ -n "$KMOD_TYPE" ]; then`

`CONF=/opt/etc/ipkg/${KMOD_TYPE}.conf`

`KMOD_CONF=/opt/etc/ipkg/${KMOD_TYPE}-kmod.conf`

`KMOD_FEED=http://ipkg.nslu2-linux.org/feeds/optware/➥`

`${KMOD_TYPE}/cross/unstable`

`else`

`CONF=/opt/etc/ipkg/${TYPE}.conf`

`fi`

`pkg_install(){`

`INSTALL_PATH="${SYS_QPKG_DIR}/tmp/install"`

`$CMD_MKDIR -p $INSTALL_PATH`

`cd $INSTALL_PATH`

`$CMD_ECHO "Downloading Optware/Ipkg ..."`

`ipk_name=$($CMD_WGET -qO- $FEED/Packages | $CMD_AWK '/^Filename: ipkg-opt/`

`{print $2}')`

`$CMD_ECHO "$CMD_WGET ${FEED}/${ipk_name}"`

`$CMD_WGET -t 5 -T 20 ${FEED}/${ipk_name} || err_log "Cannot download ➥`

`${FEED}/${ipk_name}."`

`$CMD_ECHO "Sym-link /opt ..."`

`$CMD_RM -f /opt`

`$CMD_LN -s ${SYS_QPKG_DIR} /opt`

`$CMD_ECHO "Installing Optware/Ipkg ..."`

`$CMD_TAR -xOvzf ${ipk_name} ./data.tar.gz | $CMD_TAR -C / -xzvf -2>/dev/null`

`$CMD_MKDIR -p /opt/etc/ipkg && $CMD_ECHO "src/gz $TYPE $FEED" > $CONF`

`[ -z "$KMOD_TYPE" ] || $CMD_ECHO "src $KMOD_TYPE $KMOD_FEED" >$KMOD_CONF`

`$CMD_ECHO "Export /opt/bin to $PATH ..."`

`export PATH=$PATH:/opt/bin:/opt/sbin`

`$CMD_ECHO "Updating the latest ipkg feeds ..."`

`/opt/bin/ipkg update`

`$CMD_ECHO "Deleting installation directory ..."`

`cd -$CMD_RM -r $INSTALL_PATH`

`# enable NTP & specify NTP server and time interval`

`NTP_ENABLED=$($CMD_GETCFG NTP "USE NTP Server" -d FALSE)`

`if [ "$NTP_ENABLED" != "TRUE" ]; then`

`$CMD_SETCFG NTP "USE NTP Server" TRUE`

`$CMD_SETCFG NTP "NTP Server IP" "pool.ntp.org"`

`$CMD_SETCFG NTP Interval 1`

`$CMD_SETCFG NTP TimeUnit DAY`

`/sbin/ntpdate "pool.ntp.org"`

`fi`

`$CMD_SETCFG Misc Configured TRUE`

`}`

With all files updated we can run qbuildto create the QPKG packages

`# qbuild`

`Creating archive with data files for arm-x09...`

`Creating archive with control files...`

`Creating QPKG package...`

`Creating archive with data files for arm-x19...`

`Creating archive with control files...`

`Creating QPKG package...`

`Creating archive with data files for x86...`

`Creating archive with control files...`

`Creating QPKG package...`

The QPKG packages can be found in the builddirectory.

