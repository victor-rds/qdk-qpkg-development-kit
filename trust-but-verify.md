# Trust but Verify

When a QPKG package is installed the installation script runs with super-user privileges and a maliciouspackage could cause all kinds of problems. As long as the QPKG is downloaded from a trusted packagebuilder this is probably not an issue, but better safe than sorry. If the QPKG is signed then it is possible touse the --verify option to verify that it is built by the package builder and that it hasn't been modified. For thisto work the package builder's public key must have been imported in the public keyring using the --import-key option.

All imported keys can be shown using the --list-keys option and a key can be removed from the keyringusing --remove-key.

Of course, to be able to verify a package it must have been signed when it was built; this is accomplishedusing either the --sign or the --gpg-name option at build time or an existing package can have a signatureadded using --add-sign. The --add-sign option can also be used when an already signed package should bere-signed with a different key.

Note that only QPKG packages built with QDK 2.0 or later can have a signature added using the --add-signoption.

