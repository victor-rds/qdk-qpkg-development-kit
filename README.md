# Introduction

## Preface

QDK \(QNAP Development Kit\) is used to build QPKG packages. A QPKG package is a self-extracting archive with application files and meta-data. A QPKG package makes it easy for anyone to install and remove packages. It also gives a package maintainer almost total control on how the package is installed on the NAS.

The major design goal of QDK is to make it easy for the package maintainer to create a QPKG package. QDK started out as a simple modification of the first official release of the QPKG SDK, but now supersedes it. It includes many new features like architecture check at installation, support for digital signatures, different compression algorithms, a comprehensive option to check that other required QPKG packages are installed \(or that conflicting packages are not installed\), and a powerful build script.

QDK is distributed under the GPL making it completely open and available for anyone to use.

## Intended Audience

This document is about creating QPKG packages. As such, it assumes that the reader already has some previous knowledge about QPKG packages and at least know how to install and enable a QPKG package using the web interface. This document assumes that you have a basic knowledge of common shell commands. It also helps if you have some familiarity with shell scripts.

## Conventions

The following conventions are used in this document.

Italic is used to indicate files, directories, commands, and program names when they appear in the body of a paragraph.

Constant Width is used in examples to show the contents of files or the output from commands, and to indicate environment variables, keywords, function names, and other code snippets in the body of a paragraph.

Constant Bold is used in examples to show input that is typed literally by the user. \#is the shell prompt. \[ \] surrounds optional elements in a description of program syntax. The brackets themselves should never be typed. ➥ is the code-continuation character that is inserted into code when a line shouldn't be broken, but we ran out of room on the page.

