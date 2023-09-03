---
layout: post
author: opcoder0
---

## Trouble Connecting To VPN On Windows Vista After Installing Reliance NetConnect

**Applicable In India**

I started using Reliance NetConnect wireless broadband internet over the last few weeks. Everything was fine. I used to conect to a VPN over the Reliance NetConnect connection. Things were fine.

Later at home I tried using my WiFi connection (via the Linksys router). I was able to connect to the router and browse the net. However the VPN connection was never successful. I kept getting errors. I saw the error numbers 691 and 829. I struggled with this error for two days before I figured out I had to run the following commands —

```
Netcfg –u MS_PPTP
Netcfg –u MS_L2TP
```

And then,

```
Netcfg -l %windir%\inf\netrast.inf –c p –i MS_PPTP
Netcfg –l %windir%\inf\netrast.inf –c p –i MS_L2TP
```

After running these commands I am now able to connect to the VPN via my WiFi!
