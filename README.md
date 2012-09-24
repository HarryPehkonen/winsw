Download
========

The binaries are available [here] (https://github.com/HarryPehkonen/winsw/downloads "Harry's Download Area") for download. That’s compiled from this version, and is probably newer.  Use `winsw version` to see when it was compiled.


Usage
=====

During your development…

* Take winsw.exe from the distribution, and rename it to your taste (such as `myapp.exe`)
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


Configuration Syntax
====================

```xml
<service>
  <id>NginxTest</id>
  <name>NginX</name>
  <description>Just testing</description>
  <executable>"C:\Program Files\NginX\nginx.exe"</executable>
  <stopexecutable>"C:\Program Files\NginX\nginx.exe"</stopexecutable>
  <logpath>"C:\Whatever\"</logpath>
  <logmode>roll</logmode>
  <startargument>-p "C:\Whatever"</startargument>
  <stopargument>-p "C:\Whatever"</stopargument>
  <stopargument>-s stop</stopargument>
  <env name="Name" value="The Value" />
  <beeponshutdown />
  <depend>Apache2.2</depend>
  <download from="http://www.google.ca/" to="c:\temp\deleteme.html" />
</service>
```

id
==

A unique id for the service.

name
====

This appears in the "Name" column in the Services list.

description
===========

This appears in the "Description" column in the services list.

executable
==========

Path and filename to the executable that starts the service.  Quotes get removed.

stopexecutable
==============

Optional.  If this is specified, it (along with the stop-arguments) is used to shut down the service.

logpath
=======

Optional.  Defaults to same path as `executable`.  Directory name where log files are created.

logmode
=======

Defaults to `append`.

rotate
------

Size-based log roller

reset
-----

Resets the log file every time the service is started.  Doesn't roll.

roll
----

Rolls the log file every time the service is started.

roll-by-time
------------

Must specify `pattern`.  May specify `period` (defaults to one).

I have no idea what kind of patterns to use.

roll-by-size
------------

You may specify `sizeThreshold` in kilobytes (defaults to 10 kB).

You may specify number of `keepFiles`.  Defaults to 8.

append
------

Always appends to the end of a log file.

depend
======

startargument
=============

Arguments that will be supplied to `executable`.  You can specify multiple startarguments -- they will be concatenated.

stopargument
============

Arguments that will be supplied to `stopexecutable`, or `executable` if it wasn't specified.  You can specify multiple stoparguments -- they will be concatenated.

env
===

Specify the name and value of one environment value as attributes.  Example:

`<env name="PATH" value="C:\TEMP" />`

beeponshutdown
==============

The wrapper makes a beep when the service has shut down.  It doesn't beep on restart.

download
========

Use this to automatically download any files when the service is started.

Deferred File Operations
========================

To support self updating services, winsw offers a mechanism to perform file operations before a service start up. This is often necessary because Windows prevents files from overwritten while it’s in use.

To perform file operations, write a text file (in the UTF-8 encoding) at `myapp.copies` (that is, it’s in the same directory as `myapp.xml` but with a different file extension), and for each operation add one line:

To move a file, write “src>dst”. If the ‘dst’ file already exists it will be overwritten.
The success or failure of these operations will be recorded in the event log.

Once done, the myapp.copies file will be deleted.
