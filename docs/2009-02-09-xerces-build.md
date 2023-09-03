---
layout: post
author: opcoder0
---

## Build Steps - Xerces 2.7.0 and Xalan 1.10 (32-bit) Using Visual Studio 2005 (VC8)

I read numerous posts and mailing list threads before I could figure out how to build Xalan 1.10 with Xerces 2.7.0 on Windows 32-bit OS using Visual Studio 2005 (VC 8).

There are bits and pieces of instructions here and there which add up. But I thought none of them were complete and easily accessible.

So thought of composing the instruction set here.

### Instructions for Building Xerces 2.7.0

1. First of all get the Xerces 2.7.0 sources from the apache xerces-c archive page here [http://archive.apache.org/dist/xml/xerces-c/Xerces-C_2_7_0/](http://archive.apache.org/dist/xml/xerces-c/Xerces-C_2_7_0/). Goto ‘source’ folder and download xerces-c-src_2_7_0.zip.
2. Extract it under a suitable location on your machine. (Say – C:\dev).
3. Under the extract, goto – (C:\dev) \xerces-c-src_2_7_0\Projects\Win32\
4. Open the solution file under Projects\Win32\VC7.1\xerces-all.sln. The Visual Studio 2005 will ask if the solution can be upgraded. Use the wizard to upgrade the solution.
5. Now in the opened solution, make sure you choose “Release” build.
6. Right-click on ‘XercesLib’ and click Build.
7. This should build the project without any errors. You will be able to see the built files under – (C:\dev) xerces-c-src_2_7_0\Build\Win32\VC7.1\Release. The Xerces DLL and .lib files can be found here.

**Note:** Unlike the instructions in given in Nabble, I have’nt changed the project setting (XercesLib Project Properties -> C++ -> Language -> “Treat wchar_t as built-in type”) to “No”. I have left it as the default which is “Yes”.

Above steps will complete the Xerces 2.7.0 build.

### Instructions for Building Xalan 1.10

**Xalan needs initial setup : Instructions for that here**

Now before building Xalan, set your `XERCESCROOT` environmental variable to the location where you’ve successfully built Xerces-C above (i.e. – C:\dev\xerces-c-src_2_7_0).

NOTE – My build instructions won’t work if you download the Xalan-C sources from apache download site. In order to build Xalan 1.10 with Xerces 2.7.0 you need some fixes in Xalan. And these fixes can be applied only on the SVN revision number 357825.

1. Setup a SVN client, I chose Tortoise SVN. You can download it from here. Tortoise SVN integrates into Windows explorer. So choose an appropriate check-out (SVN update) location for the Xalan sources.  Like say – C:\dev\xml-xalan.
2. Now Open Windows Explorer, choose ‘xml-xalan’ and perform a source checkout from the SVN Xalan Trunk. The trunk URL is – http://svn.apache.org/repos/asf/xalan/c/trunk
3. Now Right-click on ‘xml-xalan’ folder, choose “Tortoise SVN” -> “Update to revision” menu option. This will pop-up a dialog and ask you to provide the revision of the checkout. Here enter the revision number 357825.
4. This will checkout the entire tree with the required set of sources of Xalan 1.10. Now to this you will have to apply a source patch found under Apache Xalan’s JIRA case – http://issues.apache.org/jira/browse/XALANC-584. Download the patch file (patch.txt) from there and save it on your machine. Rename it to xalan.patch.
5. Now, in windows explorer, right-click on (C:\dev\xml-xalan) “xml-xalan” folder and choose “Tortoise SVN” -> “Apply Patch…” menu item. This will ask for the patch file. Choose the patch file xalan.patch downloaded previously. This will open the Tortoise Merge for each of the sources changes. Save the merged / patched file.

Now, we can build the Xalan 1.10 properly with Xerces 2.7.0.

1. Now, open Xalan solution file using Visual Studio 2005 from under your extract or if you are following the above path structure then it would be – C:\dev\xml-xalan\Projects\Win32\VC7.1. Visual Studio will try to perform an upgrade. Follow the wizard and complete the upgrade.
2. Make sure you’ve chosen “Release” configuration for Xalan.
3. I’ll again follow only some steps from Hans Smit’s instructions on Nabble.
4. Here, goto C:\dev\xml-xalan\Projects\Win32\VC7.1\Utils\Localization and edit ‘tools.ini’. Comment out [NMAKE].
5. Then, in BuildMessages.mak under the same location as tools.ini Add the line “include tools.ini”. Added it to the top of the file.
6. Now in Visual Studio 2005, right-click on “Localization” project and say build. This should build the MsgCreator.exe without issues.
7. Now right-click on “AllInOne” and say build. This should build the Xalan libraries.

Hope this helps !.
