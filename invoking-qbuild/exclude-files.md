# Exclude Files

Sometimes there might be files that shall be excluded from the QPKG package and by using the --excludeoption it is possible to exclude files matching the specified pattern. This option is passed on to rsync andfollows the same rules as rsync's --exclude option. Only one exclude pattern per option, but we can repeat theoption on the command line to add multiple patterns. The --exclude-from option is related to the --excludeoption, but specifies a file that contains exclude patterns \(one per line\). This option is also passed on to rsyncand follows the same rules as rsync's --exclude-from option.

