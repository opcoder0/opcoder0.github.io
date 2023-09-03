---
layout: post
author: opcoder0
---

## Configure Eclipse As TCL/TK IDE

To configure Eclipse as a TCL/TK IDE you would need the plugin DLTK (Dynamic Language Toolkit). Information on DLTK is available [http://www.eclipse.org/dltk/](http://www.eclipse.org/dltk/)

DLTK supports other scripting languages such as PERL, PHP, TCL/TK etc. Things you would need to setup TCL/TK Eclipse IDE –

- Eclipse 3.4 or greater: Java or JavaEE IDE
- DLTK
- TCL/TK
- Komodo Remote Debugger

Steps to follow are –

- Download and install Eclipse 3.4 or greater from eclipse.org (install the Java or JavaEE IDE).
- To install the DLTK, in your eclipse IDE navigate the menu item “Help > Software Updates…”, or “Help > Install New Software…” to install plugins/updates.
- Add this as the update site for DLTK – [http://download.eclipse.org/technology/dltk/updates-dev/1.0/](http://download.eclipse.org/technology/dltk/updates-dev/1.0/)
- After the site has been added it would show the list of plugins available under DLTK. Choose the following –
  - Dynamic Language Toolkit – Core Frameworks
  - Dynamic Language Toolkit – Core Frameworks SDK
  - Dynamic Language Toolkit – iTCL Development Tools
  - Dynamic Language Toolkit – iTCL Development Tools SDK
  - Dynamic Language Toolkit – TCL Development Tools
  - Dynamic Language Toolkit – TCL Development Tools SDK
  - Dynamic Language Toolkit – XOTcl Development Tools
  - Dynamic Language Toolkit – XOTcl Development Tools SDK
- After you have selected all the above packages and agreed to the license agreement, install them.
- After the install of the components, eclipse will re-start and the TCL environment (perspectives, windows etc.) would have been configured into eclipse.
- Now download and install TCL Shell and interpreter itself. For Windows, you can download it from ActiveState [http://www.activestate.com/activetcl/](http://www.activestate.com/activetcl/) and for Linux download it from www.tcl.tk/software/tcltk/
- After you have installed TCL in your preferred path/location; Open Eclipse IDE and follow the menu option “Window > Preferences > TCL“; Click on “Interpreters“, click Add button on the right, enter interpreter name as “TCL” and click “Browse..” and choose the path to the Tcl interpreter executable. Click OK and save the setting.
- Now you are done. You can write and run TCL programs via Eclipse.
- To debug your Tcl programs you would need to install the Komodo remote debugger.
- You can download Komodo from activestate – [http://aspn.activestate.com/ASPN/Downloads/Komodo/RemoteDebugging](http://aspn.activestate.com/ASPN/Downloads/Komodo/RemoteDebugging)
- Install the above software under a directory of your choice.
- Now, open eclipse, and navigate to the following menu option "Window > Preferences > TCL", expand TCL and then choose "Debug > Engines > Active State". Under the "Paths" tab, select the "Path:" dropdown and under "External Debugging Engine" select browse and choose the executable for komodo remote debugger (for windows it would be dbgp_tcldebug.exe). Click OK and save the setting.
- Now you are all set. You should be able to choose new TCL project, write your Tcl code and debug your programs.
