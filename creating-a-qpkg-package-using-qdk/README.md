# Creating a QPKG Package Using QDK

Building a QPKG package using QDK is quite easy, especially if the software is platform independent andno binary has to be built or if a tar archive with the binary files is already available.

Normally, QDK enters the process when the binary has already been built, but by using some of QDK's moreadvanced features it is possible to include the build of the binary as part of the steps performed whenbuilding a QPKG package.

To learn more about QDK we start by creating a simple platform-independent QPKG package. Next wecreate QPKG packages for different platforms using some of QDK's advanced features. Finally, we convertan existing QPKG package to QDK.

