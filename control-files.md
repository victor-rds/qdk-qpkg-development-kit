# Control Files

The control files include files as the generic and package specific installation scripts \(qinstall.shand

package\_routines\), the QPKG configuration file \(qpkg.cfg\),and the optional files with MD5 checksums and

configuration files. When the QPKG package is built these files are added to a gzip compressed tar archive,

which is added to a uncompressed tar archive as a workaround to limitations in the available busybox tools.

The header script extracts them to the installation directory.

