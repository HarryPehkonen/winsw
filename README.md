Download
========

The binaries are available [here] (https://github.com/HarryPehkonen/winsw/downloads "Harry's Download Area") for download. That’s compiled from this version, and is probably newer.  Use `winsw version` to see when it was compiled.


Usage
=====

During your development…

* Take winsw.exe from the distribution, and rename it to your taste (such as myapp.exe)
* Write `myapp.xml` (see Configuration Syntax for more details)
* Place those two files side by side when you deploy your application, because that’s how winsw.exe discovers its configuration.

At runtime…

* To install a service, run myapp.exe install
* To start a service, run myapp.exe start
* To stop a service, run myapp.exe stop
* To restart a service, run myapp.exe restart
* To uninstall a service, run myapp.exe uninstall

When there’s a problem, these commands also report an error message to stderr. On a successful completion, these commands do not produce any output, and exit with 0.

In addition, you can also run `myapp.exe` status to have it print out the current status of the service to stdout. Again, any error encountered during the processing would cause output to be reported to stderr.

All these commands use the same set of exit code to indicate its result.


Deferred File Operations
========================

To support self updating services, winsw offers a mechanism to perform file operations before a service start up. This is often necessary because Windows prevents files from overwritten while it’s in use.

To perform file operations, write a text file (in the UTF-8 encoding) at `myapp.copies` (that is, it’s in the same directory as `myapp.xml` but with a different file extension), and for each operation add one line:

To move a file, write “src>dst”. If the ‘dst’ file already exists it will be overwritten.
The success or failure of these operations will be recorded in the event log.

Once done, the myapp.copies file will be deleted.
