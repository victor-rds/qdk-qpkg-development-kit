# Data File

This is the compressed \(gzip, bzip2, or 7-zip\) tar archive with the actual data files which was created at build

time with the files from the build root directory. The compressed tar archive is extracted to the installation

directory and the installation script uncompress and extract the content to the QPKG directory. When the

data archive is extracted to the installation directory some redundant data is included at the end of the data

archive for performance reasons. The \/bin\/dd command on QNAP devices doesn't support different block

sizes for the input and output, so to be able to extract the data at a reasonable speed the block size is set to

1024 bytes for both input and output. This gives the result that some extra data is added to the end of the

archive \(up to 1023 bytes depending on the original size of the archive\). This doesn't matter since the tar

archive includes an EOF marker and can be extracted without problems.

