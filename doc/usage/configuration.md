# Configuration

> Whenever OpenOffice.org (OOo for short) is mentioned, this can generally be interpreted to include any office suite derived from OOo such as [Apache OpenOffice](https://www.openoffice.org) and [LibreOffice](https://www.libreoffice.org).

When using a DefaultOfficeManager, there are a number of settings that can be configured. Some of the default settings used by JODConverter have been chosen because they have a greater chance of working out of the box, but they are not necessarily the optimal ones.

### Port Numbers / Pipe Names

OOo inter-process communication can use either TCP sockets and/or named pipes. The default is to use a TCP socket, on port 2002.

Named pipes have the advantage of not taking up TCP ports (with their potential security implications), and they are marginally faster. However they require a native library to be loaded by the JVM, and this means having to set the java.library.path system property. That's why it's not the default.

The path that needs to be added to java.library.path is different depending on the platform, but it should be the directory in the OOo installation containing libjpipe.

- On Linux it's e.g.: java -Djava.library.path=/opt/openoffice.org/ure/lib
- On Windows it's e.g.: java "-Djava.library.path=C:\Program Files (x86)\OpenOffice 4\program"

### Office Home

Specifies the office home directory of the office installation that will be used to perform document conversions.

By default the office home is auto-detected, starting with LibreOffice and the most recent version. But you can force JodConverter to use a a specific OOs installation.

- On Linux it's e.g.: java -Djava.library.path=/opt/openoffice.org
- On Windows it's e.g.: java "-Djava.library.path=C:\Program Files (x86)\OpenOffice 4"

### Process Manager

A process manager is used when JODConverter needs to deal with a started office process. When JODConverter starts an office process, it must retrieve the PID of the started process in order to be able to kill it later if required.

By default, JODConverter will try to find the best process manager according to the OS on which JODConverter is running. But any process manager implementing the ProcessManager interface can be used if found on the classpath.

### Template Profile Dir

The OfficeManager creates a temporary profile dir for its OOo process, to avoid interfering with e.g. another OOo instance being used by the user.

By default this temporary profile will be a new one created by OOo with its own defaults settings, and relies on the [-nofirststartwizard](https://wiki.openoffice.org/wiki/Framework/Article/Command_Line_Arguments) command line option.

You may want to provide a templateProfileDir containing customized settings instead. The OfficeManager will copy such template dir to the temporary profile, so OOo will use the same settings while still keeping the OOo instances separate.

The profile can be customized in OOo by selecting the Tools > Options menu item. Settings that may be worth customizing for automated conversions include e.g.

- Load/Save > General: you may e.g. want to disable "Save URLs relative to internet" for security reasons
- Load/Save > Microsoft Office: these options affect conversions of embedded documents, e.g. an Excel table contained in a Word document. If not enabled, the embedded table will likely be lost when converting the Word document to another format.