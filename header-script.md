# Header Script

If the QPKG file is built for a specific architecture then QDK adds an architecture check to the header. The

rest of the header script is code to create the directory for the files and to extract them. The header script is

generated at build-time by the qbuildapplication.

Example of header script.

`#!/bin/sh`

`wrong_arch(){`

`local wrong_arch_msg="Wrong architecture: Optware 0.99.163 is built for arm-x19"`

`echo "Installation Abort." && echo "$wrong_arch_msg"`

`/sbin/log_tool -t2 -uSystem -p127.0.0.1 -mlocalhost -a "$wrong_arch_msg"`

`echo -1 > /tmp/update_process && exit 1`

`}`

`arch_ok(){`

`local cpu_arch=$(/bin/uname -m)`

`[ $(/usr/bin/expr match "$cpu_arch" "armv5tel") -ne 0 ] || return 1`

`}`

`/bin/echo "Install QNAP package on TS-NAS..."`

`/bin/grep "/mnt/HDA_ROOT" /proc/mounts >/dev/null 2>&1 || exit 1`

`arch_ok || wrong_arch`

`_EXTRACT_DIR="/mnt/HDA_ROOT/update_pkg/tmp"`

`/bin/mkdir -p $_EXTRACT_DIR || exit 1`

`script_len=1032`

`/bin/dd if=${0} bs=$script_len skip=1 | /bin/tar -xO | /bin/tar -xzv -C $_EXTRACT_DIR ➥`

`|| exit 1`

`offset=$(/usr/bin/expr $script_len + 10240)`

`/bin/dd if=${0} bs=$offset skip=1 | /bin/cat | /bin/dd bs=1024 count=11 ➥`

`of=$_EXTRACT_DIR/data.tar.gz || exit 1`

`offset=$(/usr/bin/expr $offset + 10598)`

`( cd $_EXTRACT_DIR && /bin/sh qinstall.sh || echo "Installation Abort." )`

`/bin/rm -fr $_EXTRACT_DIR && exit 10`

`exit 1`



