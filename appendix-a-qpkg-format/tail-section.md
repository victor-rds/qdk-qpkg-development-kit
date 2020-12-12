# Tail Section

The tail section contains QNAP specific data that is used by the web interface at installation. It consists of

100 bytes, where the first 10 includes the model \(QDK\_BUILD\_MODEL\), followed by 40 unspecified reserved

bytes \(some of them are used for the checksum value\), 10 bytes reserved for the firmware version \(not

supported in QDK\), 20 bytes for the name \(QPKG\_NAME\), 10 bytes for the version \(QPKG\_VER\), and finally a

10 bytes flag area, which shall be set to "QNAPQPKG " for QPKG packages.

