# Status Information

The application normally outputs errors and progress information to the terminal, but by using the -q or--quiet option we can change this. In quiet mode nothing is written to standard output. Normally only errormessages are displayed. If quiet mode has been specified as the default mode then the -v or --verbose optioncan be used to enable normal output. By repeating the -v option the verbosity is increased. The maximum is 3\(4 if quiet mode has been specified as the default mode.\)

By default only errors terminate the ongoing build process, while warnings are written to the terminal, butstill allows the build process to continue. If the --strict option is specified then all warnings are treated aserrors.

