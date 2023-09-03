---
layout: post
author: opcoder0
---

## Windows Installer: MSI, MSP, MST, .CAB

I recently had to understand and debug an application that processes MSI and MSP files. This was my first time I had to really work on debugging Windows Installer application. And a learnt a thing or two which I found useful. Then I thought it would be blog about the basics.

- MSI – This is a Windows installer package (a .msi file) which is used for installation, repair of software on Windows OSes. A MSI file can be viewed and editied by using softwares like Wise for Windows Installer (now Symantec), ORCA a tool that Microsoft ships with its Windows Installer SDK (which can also be downloaded for free here). You can imagine a MSI to contain a set of Properties, Tables (like database tables), Components, Files, Custom Action logic etc. In Windows Installer files could be inside the MSI or could be part of an “Administrative Install”. In an Administrative installation, the files are kept in a separate location and the MSI contains the location information of the files which are used during installation.
- MSP – In a nutshell (MSP = MST + .CAB). A patch file (.msp) contains MST (Transforms) and CAB (Cabinet) files. The files in the patch could be patching existing files which are part of the base installation and/or could contain a set of new files not present in the original MSI. The files in the .CAB in the MSP could contain only binary differences as compared to the files in the base installation. Windows installer follows a set of rules to decide whether a file in the patch is a diff or if it has to keep the entire file. If the size of the file part of the patch is greater than the size of the original file, then a diff is created. But if the size of the file is smaller, then the entire file is maintained. Look at this link for more information — [http://msdn.microsoft.com/en-us/library/aa371146(VS.85).aspx](http://msdn.microsoft.com/en-us/library/aa371146(VS.85).aspx).
- MST – As we know a MSI contains properties, tables etc. If the patch is changing any of the properties or one of the values in a table such as updating a file’s timestamp etc.; then such things are recorded as a transform. If you have an MST file, then you could observe the effects it has on the MSI by applying the transform to the MSI. You could achieve this by using the ORCA tool. Open the MSI using ORCA, then click on menu Transform -> Apply Transform…

**Installing a MSI**

The command to install, un-install, verify, update a MSI/Package is -> “msiexec.exe”. To install an msi, you could use the command –

```
msiexec /i adobe.msi /L*v msi_install.log
```

**Enable Logging**

When debugging or looking into errors that may have been caused during installation/uninstallation etc, looking at a detailed log is most important. The above command will install the MSI with verbose logging. Inorder for verbose logging to work, you will have to add the following registry setting –

- run ‘regedit’
- Goto `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer`
- Create key Logging (Type will be REG_SZ).
- For value set `voicewarmupx`
- For complete  on logging see – [http://support.microsoft.com/kb/223300](http://support.microsoft.com/kb/223300). Also look at the Wikipedia link [http://en.wikipedia.org/wiki/Windows_Installer](http://en.wikipedia.org/wiki/Windows_Installer) that has additional info on how to enable this logging from Windows Server 2008 onwards (the procedure has changed as compared to how it was done in XP/W2K3).

**More on Patching**

If you want to extract a .msp file and look in to what is inside the file then you could use a tool called msix to extract a MSP file into pieces. The msix tool extracts an MSI into a set of MSTs and CAB files.The tool along with the source has been provided along with good description on Heath Stewarts MSDN blog link – http://blogs.msdn.com/heaths/archive/2006/04/07/571138.aspx

Usually as a good practice exe and dlls have version information associated with them. So a exe/dll in a patch would have a version greater than the one present in the base installation. In such  cases it is easier for Windows installer to decide how to patch the file. But in some cases, where the exe/dll does not have any version information or if the file is not an exe/dll. For example if it is a text file or some other file like an image for example. Then Windows installer follows a flowchart to decide how to patch such files. I found a good link here – Aaron Stebner’s WebLog : How Windows Installer handles file replacement logic for versioned and unversioned files

Programming –

You could write a program and register a call back as an external UI, which gets called for every file being installed by the Windows installer. See this API as a starting point for that here documented here – http://msdn.microsoft.com/en-us/library/aa370384(VS.85).aspx

Windows Installer page has a flowchart that clearly describes the handling of such scenarios – http://msdn.microsoft.com/en-us/library/aa370532%28VS.85%29.aspx

Here are a bunch of reference links that helped me understand some basics of Windows Installer

- Windows Installer – Wikipedia, the free encyclopedia [http://en.wikipedia.org/wiki/Windows_Installer](http://en.wikipedia.org/wiki/Windows_Installer)
- Windows Installer Links — ServerWatch.com [http://www.serverwatch.com/tutorials/article.php/1548271/Windows-Installer-Links.htm](http://www.serverwatch.com/tutorials/article.php/1548271/Windows-Installer-Links.htm)
- How to enable Windows Installer logging [http://support.microsoft.com/kb/223300](http://support.microsoft.com/kb/223300)
- Patching (Windows) [http://msdn.microsoft.com/en-us/library/aa370578%28VS.85%29.aspx](http://msdn.microsoft.com/en-us/library/aa370578%28VS.85%29.aspx)
- Patch Packages (Windows) [http://msdn.microsoft.com/en-us/library/aa370596%28VS.85%29.aspx](http://msdn.microsoft.com/en-us/library/aa370596%28VS.85%29.aspx)
- Aaron Stebner’s WebLog : How Windows Installer handles file replacement logic for versioned and unversioned files [http://blogs.msdn.com/astebner/archive/2005/08/30/458295.aspx](http://blogs.msdn.com/astebner/archive/2005/08/30/458295.aspx)
- Ask the Performance Team : Two Minute Drill: The .MSP file Property Reference (Windows) [http://blogs.technet.com/askperf/archive/2009/05/26/two-minute-drill-the-msp-file.aspx](http://blogs.technet.com/askperf/archive/2009/05/26/two-minute-drill-the-msp-file.aspx)
- MSI Property reference – [http://msdn.microsoft.com/en-us/library/aa370905%28VS.85%29.aspx](http://msdn.microsoft.com/en-us/library/aa370905%28VS.85%29.aspx)
