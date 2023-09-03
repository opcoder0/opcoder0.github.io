---
layout: post
author: opcoder0
---

## Enable Process Dump In Windows

It is hard to believe how complex enabling user-dump can get in Windows !! And there are different procedures on every different version of Windows ! I don’t understand Windows internals that well. But I wonder what makes it that difficult to keep it consistent across their own OS kernels. Gives one more reason (as a developer) to not like Windows. And there are different tools, procedures, scattered knowledge base articles to illustrate the steps.

Using a tool named `userdump` and this can be used to generate user dump on the following Windows OSes –

- Microsoft Windows Server 2003, Standard x64 Edition
- Microsoft Windows Server 2003, Enterprise x64 Edition
- Microsoft Windows Server 2003, Standard Edition (32-bit x86)
- Microsoft Windows Server 2003, Enterprise Edition (32-bit x86)
- Microsoft Windows XP Professional x64 Edition
- Microsoft Windows XP Professional
- Microsoft Windows XP Home Edition
- Microsoft Windows 2000 Service Pack 1
- Microsoft Windows 2000 Service Pack 2
- Microsoft Windows 2000 Advanced Server

A sample of using userdump – (@See more here http://support.microsoft.com/kb/250509)

```
C:\> userdump 1188 c:\store.dmp
```

Now this is different for Windows Vista and Windows 7 and  for these OS flavors, you open task manager, right-click on the process and click on "Create dump file" option. And if you have process that hangs or crashes you can use `Adplus` tool. The same procedure is applicable to all sorts of Windows 2008 editions. Adplus is a vbscript a utility written around CDB. This can run in two modes –

- Hang mode – to detect process hangs and create user dump
- Crash mode – to detect process crashes and create user dump

For example to create a user dump using adplus run the following command –

For a process that crashes –

```
C:\> adplus -crash -p <PID>
```

For the process that hangs –

```
C:\> adplus -hang -p <PID>
```

Here is a decent link I found that records a lot of these options – [http://support.microsoft.com/kb/286350/](http://support.microsoft.com/kb/286350/)

And then there is “Debug diagnostic 1.1” that is used to debug high performance applications that crashes, hangs etc. [http://support.microsoft.com/kb/931370](http://support.microsoft.com/kb/931370)

And then there is generating a Kernel dump. Luckily there is just one way to generate kernel dump on most of the Windows OSes; Details on how to is here [http://support.microsoft.com/kb/254649](http://support.microsoft.com/kb/254649); There is a hotfix from Microsoft that allows user to generate memory dump via a keystroke – [http://support.microsoft.com/kb/244139](http://support.microsoft.com/kb/244139)

Here are some reference links

- [http://www.carnal0wnage.com/papers/userinfouserdump.pdf](http://www.carnal0wnage.com/papers/userinfouserdump.pdf)
- [http://www.hammerofgod.com](http://www.hammerofgod.com)

Now what about production systems where you wouldn’t want to install any such tools but still want to get a user-dump file. This is still a question to me and am looking for an answer. So if any of you windows gurus are here do tell me how.
