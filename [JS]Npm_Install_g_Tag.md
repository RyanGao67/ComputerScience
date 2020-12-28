### Install locally vs Install globally
* If you’re installing something that you want to use in your program, using require('whatever'), then install it locally, at the root of your project.
* If you’re installing something that you want to use in your shell, on the command line or something, install it globally, so that its binaries end up in your PATH environment variable.

### Of course, there are some cases where you want to do both. 
* Coffee-script and Express both are good examples of apps that have a command line interface, as well as a library. 

* Install it in both places. Seriously, are you that short on disk space? It’s fine, really. They’re tiny JavaScript programs.
