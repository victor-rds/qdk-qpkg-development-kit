# Creating a Simple QPKG Package

The first simple QPKG package we create is in fact, the QDK itself.

To create a new QPKG package from scratch we can use qbuild to create a template directory with the basicstructure.

`# qbuild --create-env QDK`

The command creates a directory named QDK with the contents from the template directory

> arm-x19\/
>
> config\/
>
> icons\/
>
> shared\/QDK.sh
>
> x86\/
>
> x86\_64\/
>
> package\_routines
>
> qpkg.cfg

The arm-x09, arm-x19, x86, and x86\_64 directories are only used when building platform specific QPKG packages, so we remove those directories. Neither are we including any configuration files, so the configdirectory is also removed. The template init-script, QDK.sh, will be replaced by the one from the QDKinstallation directory, so it is also removed.

Next, we populate the remaining directories, shared and icons, with content. In this case, all the files areavailable in the QDK installation directory \(as described in the section about the installation of QDK\), so it isonly a matter copying them to the directories \(this example is on a RAID volume\)

\#rmdir arm-x09 arm-x19 x86 x86\_64 config

\#rm shared\/QDK.shcp -pr \/share\/MD0\_DATA\/.qpkg\/QDK\/\* shared

\#cp -p \/share\/MD0\_DATA\/.qpkg\/QDK\/.qpkg\_icon.gif icons\/QDK.gif

\#cp -p \/share\/MD0\_DATA\/.qpkg\/QDK\/.qpkg\_icon\_80.gif icons\/QDK\_80.gif

\#cp -p \/share\/MD0\_DATA\/.qpkg\/QDK\/.qpkg\_icon\_gray.gif icons\/QDK\_gray.gif

The directory structure should now look like this

> icons\/
>
> ```text
>       QDK.gif
>
>       QDK\_80.gif
>
>       QDK\_gray.gif
> ```
>
> shared\/
>
> ```text
> bin\/
>
>      qbuild
> ```
>
> scripts\/
>
> ```text
>      qinstall.sh
> ```
>
> template\/
>
> ```text
>      arm-x09\/
>
>      arm-x19\/
>
>      config\/
>
>      icons\/
>
>      shared\/
>
>          init.sh
>
>      x86\/
>
>      x86\_64\/
>
>      package\_routines
>
>      qpkg.cfg
>
> qdk
> ```
>
> package\_routines
>
> qpkg.cfg

The QPKG configuration file, qpgk.cfg, was created with default settings \(the author is by default set to thename of the user running qbuild.\)

> QPKG\_NAME="QDK"
>
> QPKG\_VER="0.1"
>
> QPKG\_AUTHOR="micke"
>
> QPKG\_SERVICE\_PROGRAM="QDK.sh"
>
> QPKG\_RC\_NUM="101"

We update the file to include a valid setting for the service program, qdk, specify the automatically generatedconfiguration file that should be restored at an upgrade, license, one-line description, and also a differentversion number. All unused settings are removed leaving us with the following content.

> QPKG\_NAME="QDK"
>
> QPKG\_VER="2.0"
>
> QPKG\_AUTHOR="micke"
>
> QPKG\_LICENSE="GPLv2+"
>
> QPKG\_SUMMARY="QDK \(QPKG Development Kit\) is used to create QPKG packages."
>
> QPKG\_SERVICE\_PROGRAM="qdk"
>
> QPKG\_RC\_NUM="181"
>
> QPKG\_CONFIG="\/etc\/config\/qdk.conf"

When QDK is installed we want the installation script to create a default system-wide configuration file in\/etc\/config\/qdk.conf. We could include a default file in shared and used that file at installation, but generatingit at installation provides a more flexible solution. It also gives us an opportunity to introduce thepackage\_routines file.At installation we have to create the system-wide configuration file and then at a future upgrade we have tomodify the settings in the existing configuration file, so we add a definition with the path to the system-wideconfiguration file at the top and script code to modify the file to pkg\_install.

QDK\_CONF=\/etc\/config\/qdk.conf

pkg\_install\(\){

> if \[ -f "$QDK\_CONF" \]; then
>
> ```text
>              $CMD\_SED -i "s!\\(QDK\_VERSION=\\).\*!\1$QPKG\_VER!" $QDK\_CONF
>
>              $CMD\_SED -i "s!\\(QDK\_PATH=\\).\*!\1$SYS\_QPKG\_DIR!" $QDK\_CONF
> ```
>
> else
>
> ```text
>              $CMD\_ECHO "QDK\_VERSION=$QPKG\_VER" &gt; $QDK\_CONF
>
>              $CMD\_ECHO "QDK\_PATH=$SYS\_QPKG\_DIR" &gt;&gt; $QDK\_CONFfi
>
>              $CMD\_CHMOD 644 $QDK\_CONF}
> ```

When the package is removed the system-wide configuration file should also be removed, so a main removefunction is also included,

> PKG\_MAIN\_REMOVE="{
>
> ```text
>             $CMD\_RM -f $QDK\_CONF
> ```
>
> }"

The addition of QPKG\_CONFIG="\/etc\/config\/qdk.conf" to qpkg.cfg makes sure that any local modifications are not lost at an upgrade. When qbuild is run it automatically adds a static checksum value forthe configuration file to the md5sum file to handle this, however, we must use the --force-config option toindicate that the missing configuration file should not be treated as an error. Since the QDK 1.0 version alsoincluded this configuration file, but at that time no support existed for specifying configuration files, theconfiguration file is going to be handled as an unknown configuration file at the first upgrade, which wouldresult in it being replaced by a new file \(and the current file would be saved in \/etc\/config\/qdk.conf.qdkorig.To avoid this we use the package specific initialization function to specify that we trust the current file andthat it can be handled as if it was a known configuration file. We accomplish this by adding the file to\/etc\/config\/qpkg.conf and setting the md5sum value to 0. This makes the installation script handle theconfiguration files as an already known file and the current file is untouched.

> pkg\_init\(\){
>
> ```text
>                add\_qpkg\_config $QDK\_CONF 0
> ```
>
> }

With all files updated we can run qbuild to create the QPKG package

> \# qbuild --fore-config
>
> Creating archive with data files...
>
> Creating archive with control files...
>
> Creating QPKG package...

The qbuild application figures out by itself that only a platform-independent QPKG package shall be createdand the resulting QPKG package is placed in a directory named build. To change the location of where tostore the result the --build-dir option can be used,

`# qbuild --force-config --build-dir /share/QPKG/`

