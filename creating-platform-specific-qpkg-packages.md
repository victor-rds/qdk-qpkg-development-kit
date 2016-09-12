# Creating Platform Specific QPKG Packages

Next we create platform specific QPKG packages. For this example we create QPKG packages of OneTouch-Run for x86 and ARM x19. It is a very simple application, but it can still be used to show some QDK

features that could come in handy when building for multiple architectures.

First a brief description of the development setup used in this example. The NAS itself doesn't include any

development tools \(apart from QDK\); instead there are two build servers in the local network that perform

the builds, one named huginrunning Debian on ARM and one named muninrunning Debian on x86. To

make sure that they work with pristine source code files they only work with files from a source code

repository. That is, when the QPKG package is about to be built on the NAS all source code must first be

checked in to the source code repository. This is not shown in the example, though. On the build server there

is a user named qdkthat accepts commands from remote hosts using ssh, for example to build a project. The

result can be fetched using scp.

Just like when we created a single QPKG package we start by creating a template directory with a basic

structure.

`# qbuild --create-env One-Touch-Run`

We remove the unused directories \(iconsand arm-x09\) and create a new directory, source, for the source

code to the One-Touch-Run library. We place the file with the C code in this directory together with a simple

Makefile.

otr.c

`#include <stdlib.h>`

`extern int Get_Usb_Copy_Avail();`

`int Get_Usb_Copy_Avail()`

`{`

`(void)system( "/etc/init.d/one_touch_run run-script" );`

`return 0;`

`}`

Makefile

`otr.so : otr.o`

`$(CC) -o $@ -shared $?`

`.c.o :`

`$(CC) -fPIC -c $?`

`clean :`

`$(RM) *.o *.so`

The files in the sourcedirectory are added to the source code repository to make them available to the build

servers.

To be able to enable and disable the One-Touch-Run feature we edit the template init-script in shared.

`#!/bin/sh`

`SCRIPT_DIR=/share/OTR/`

`CONF=/etc/config/qpkg.conf`

`QPKG_NAME=One-Touch-Run`

`usage()`

`{`

`echo "Usage: $0 start|stop|restart|run-script"`

`exit 1`

`}`

`case "$1" in`

`start)`

`ENABLED=$(/sbin/getcfg $QPKG_NAME Enable -u -d FALSE -f $CONF)`

`if [ "$ENABLED" != "TRUE" ]; then`

`echo "$QPKG_NAME is disabled."`

`exit 1`

`fi`

`OTR_DIR=$(/sbin/getcfg $QPKG_NAME Install_Path -d "" -f $CONF)`

`OTR_LIB=$OTR_DIR/otr.so`

`if [ -f $OTR_LIB ]; then`

`/sbin/daemon_mgr gpiod stop "/sbin/gpiod &"`

`/sbin/daemon_mgr gpiod start "LD_PRELOAD=$OTR_LIB /sbin/gpiod &"`

`else`

`echo "$OTR_LIB: No such file"`

`exit 1`

`fi`

`;;`

`stop)`

`/sbin/daemon_mgr gpiod stop "/sbin/gpiod &"`

`/sbin/daemon_mgr gpiod start "/sbin/gpiod &"`

`;;`

`restart)`

`$0 stop`

`$0 start`

`;;`

`run-script)`

`ENABLED=$(/sbin/getcfg $QPKG_NAME Enable -u -d FALSE -f $CONF)`

`if [ "$ENABLED" = "TRUE" ] && [ -d $SCRIPT_DIR ]; then`

`for script in $(/usr/bin/find $SCRIPT_DIR -type f)`

`do`

`if [ -x $script ]; then`

`$script`

`fi`

`done`

`fi`

`;;`

`*)`

`usage`

`;;`

`esac`

`exit 0`

The init-script has support for the three required commands, start, stop, restart, and also an extra command

run-scriptthat is called when the button is pushed to run the user-defined script.

To make adjustments during the build for the different platforms we use the script support in qbuild. First we

add a configuration file for the user in ~\/.qdkrc and in the DEFAULTsection we specify that if there is a

script named setup.shin the directory then it is run before the build process is initiated \(the setup phase\).

\[DEFAULT\]

QDK\_SETUP=setup.sh

By placing it in the DEFAULT section we can reuse this for any other project without adding new sections to

the configuration file every time a new project is created.

A different solution that is project specific is to add a section named OTR and then specify the scripts and

architectures in the section.

\[OTR\]

QDK\_SETUP=setup.sh

QDK\_PRE\_BUILD=build.sh

QDK\_TEARDOWN=cleanup.sh

QDK\_BUILD\_ARCH="arm-x19,x86"

In this case it would be necessary to specify the section name when running qbuild.

`# qbuild -s OTR`

The setup.shscript is used to create pre-build and teardown scripts. It also handles key management to access

the remote build servers \(that perform the actual build of the One-Touch-Run libraries for the different

platforms\) and specifies the platforms, so we don't have to do that on the command line. The pre-build script,

build.sh, is used to instruct the build servers to perform the build, fetch the library, and clean up on the build

server, while the teardown script, cleanup.sh, is used to clean up after the QPKG packages have been built.

setup.sh

`#!/bin/sh`

`QDK_BUILD_ARCH="arm-x19,x86"`

`# Initialize key management`

`eval $(/opt/bin/ssh-agent)`

`/opt/bin/ssh-add`

`QDK_PRE_BUILD=build.sh`

`QDK_TEARDOWN=cleanup.sh`

`/bin/cat >$QDK_PRE_BUILD <<EOF`

`#!/bin/sh`

`ARCH="$1"`

`HOST=`

`case "$ARCH" in`

`arm-x19)`

`HOST=hugin`

`;;`

`x86)`

`HOST=munin`

`;;`

`*)`

`HOST=`

`;;`

`esac`

`if [ -n "$HOST" ]; then`

`/opt/bin/ssh qdk@$HOST 'otr-build'`

`/opt/bin/scp qdk@$HOST:/opt/result/OTR/otr.so $ARCH`

`/opt/bin/ssh qdk@$HOST 'otr-clean'`

`fi`

`EOF`

`/bin/cat >$QDK_TEARDOWN <<EOF`

`#!/bin/sh`

`/opt/bin/ssh-add -D`

`/opt/bin/ssh-agent -k`

`/bin/rm -f arm-x19/otr.so x86/otr.so`

`/bin/rm $QDK_PRE_BUILD`

`/bin/rm $QDK_TEARDOWN`

`EOF`

The directory structure should now look like this

arm-x19\/

shared\/

one\_touch\_run

source\/

Makefile

otr.c

x86\/

package\_routines

qpkg.cfg

setup.sh

The QPKG configuration file has a simple content.

`QPKG_NAME="One-Touch-Run"`

`QPKG_VER="0.5"`

`QPKG_AUTHOR="micke"`

`QPKG_LICENSE="GPLv3+"`

`QPKG_SUMMARY="Run user-defined script using One-Touch button."`

`QPKG_SERVICE_PROGRAM="one_touch_run"`

`QPKG_RC_NUM="110"`

With all files updated we can run qbuildto create the QPKG package

`# qbuild`

Agent pid 1687

Enter passphrase for \/home\/micke\/.ssh\/id\_rsa:

Identity added: \/home\/micke\/.ssh\/id\_rsa \(\/home\/micke\/.ssh\/id\_rsa\)

cc -fPIC -c otr.c

cc -o otr.so -shared otr.o

otr.so 100% 6111 6.0KB\/s 00:00

Creating archive with data files for arm-x19...

Creating archive with control files...

Creating QPKG package...

cc -fPIC -c otr.c

cc -o otr.so -shared otr.o

otr.so 100% 5787 5.7KB\/s 00:00

Creating archive with data files for x86...

Creating archive with control files...

Creating QPKG package...

All identities removed.

unset SSH\_AUTH\_SOCK;

unset SSH\_AGENT\_PID;

echo Agent pid 1687 killed;

When qbuildis run it checks the settings in the configuration file, ~\/.qdkrc, and finds that it should run a

script named setup.shif it exists. Running this script starts the ssh agent and the private key is added to the

agent after asking for the passphrase. Using an agent for the key management means that we only have to

enter the passphrase once instead of for every sshoperation. At the same time the two scripts, build.shand

cleanup.sh, are created in the background and qbuildis also instructed to build QPKG packages for both the

x86 and arm-x19 architecture.

Before building a QPKG package for a specific architecture it first calls the defined pre-build script with the

architecture as input argument. The pre-build script calls the correct build server to build the library, fetch

the result, and cleans up on the build server. The result is then used for the QPKG package.

After QPKG packages have been built for all architectures the teardown script is called to terminate the key

management and remove any temporary files \(the built libraries and the pre-build and teardown scripts\).

The QPKG packages can be found in the builddirectory.

