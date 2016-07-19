# User Configuration File

The configuration file can be used to add customized settings for the various QDK variables. Any entries inthe file must be added to sections. Each section has a name, which is enclosed in square brackets, followedby variable definitions. A section can include different QDK prefixed variables. Configuration sections areoften useful for specific often-used groups of options. It is possible to define these options in a section of theconfiguration file and then just specify the section as the argument to qbuild.

By default, the DEFAULT section is included and then any sections specified on the command line using--section SECTION \(or -s SECTION\). Several sections can be included by repeating the option.

For example, if QDK\_PRE\_BUILD =pre\_build.sh is added to the DEFAULT section in ~\/.qdkrc, then we couldadd a script named pre\_build.sh to any project and when qbuild is run it will automatically run this script aspart of the pre-build actions. If the location of a specific project, in this example Python, is specified then itwould be possible to run 'qbuild -s python' anywhere and qbuild would still build the project at the correct location.

\[DEFAULT\] 

QDK\_PRE\_BUILD=pre\_build.sh

\[python\]

QDK\_ROOT\_DIR=\/share\/QPKG\/Python

QDK\_VERBOSE=0

It is possible to specify a different location for the user configuration file in the system-wide configurationfile, \/etc\/config\/qdk.conf, using the QDK\_USER\_CONFIG\_FILE variable. For example, if all QPKGdevelopment if performed using the admin user then it might be easier to place the configuration file on theactual HDD instead of \/root\/.qdkrc, which would be lost at each reboot.

