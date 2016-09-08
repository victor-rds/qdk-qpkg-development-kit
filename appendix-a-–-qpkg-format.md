# Appendix A – QPKG Format

Simply described a QPKG file is a self-extracting archive that consists of a header, followed by the data, and

last a tail section.

The header is a shell script that extracts the attached data and the tail section is 100 bytes of QNAP specific

data used by the web interface at installation.

In QDK the data has been split into several parts and a QDK area has been added for QDK specific data.

First a tar archive that contains the gzip compressed tar archive with the control files is attached, then the

data archive \(either a gzip, bzip2, or 7-zip compressed tar archive\), which is followed by an optional tar

archive with any extra data files \(specified using QDK\_EXTRA\_FILE\). If the QPKG file is signed then the

signature is added to the QDK area.

                                          ![](/assets/2016-09-08_171506.png)

