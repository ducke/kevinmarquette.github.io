---
layout: post
title: "Powershell: Windows Defender vs PowerShell Modules, Peasecto.A"
date: 2018-01-28
tags: [PowerShell]
---

There are reports of a Windows Defender update flagging common PowerShell tools and modules as infected with Peasecto.A. We don't have a lot of information right now other than indications that PackageManagement, PowerShellGet, MSOnline, PSScriptAnalyzer and [VSCode](https://github.com/Microsoft/vscode/issues/42284) are impacted. These are all false positives at this point.

<!--more-->
Not a lot of options other than disable defender or disable PowerShell AMSI. I'll update this post once I see more information on how this plays out. For now, I'll just share these reports in case those threads pick up traction.

# Twitter

These are the first tweets talking about the issue:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">What&#39;s this?<br>Defender real-time protection removing files from my <a href="https://twitter.com/hashtag/PowerShell?src=hash&amp;ref_src=twsrc%5Etfw">#PowerShell</a>  modules!</p>&mdash; Luc Dekens (@LucD22) <a href="https://twitter.com/LucD22/status/957732708741992448?ref_src=twsrc%5Etfw">January 28, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">FYI: Windows Defender is detecting Legit PowerShell Modules and PowerShell Core itself as PowerShell/PeaSecto.a <a href="https://t.co/i7DZEo3Pgn">https://t.co/i7DZEo3Pgn</a></p>&mdash; Mark Kraus MVP (@markekraus) <a href="https://twitter.com/markekraus/status/957743213749686272?ref_src=twsrc%5Etfw">January 28, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-cards="hidden" data-lang="en"><p lang="en" dir="ltr">That awkward moment when Windows Defender declares that your <a href="https://twitter.com/hashtag/PowerShell?src=hash&amp;ref_src=twsrc%5Etfw">#PowerShell</a> module is a threat. <a href="https://t.co/DJxvBYenJi">pic.twitter.com/DJxvBYenJi</a></p>&mdash; Boe Prox (@proxb) <a href="https://twitter.com/proxb/status/957723701071568897?ref_src=twsrc%5Etfw">January 28, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Might have overlooked this, but is there any guidance on dealing with <a href="https://twitter.com/hashtag/PowerShell?src=hash&amp;ref_src=twsrc%5Etfw">#PowerShell</a> AMSI false positives in Defender outside of DisableIOAVProtection?  Some innocuous automation being flagged, would prefer not to turn this off...</p>&mdash; Warren F. (@psCookieMonster) <a href="https://twitter.com/psCookieMonster/status/956995840006152192?ref_src=twsrc%5Etfw">January 26, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


# TechNet

Mark was talking about this thread: [Latest updates indicated Peasecto.A infection](https://social.technet.microsoft.com/Forums/en-US/40fa56dd-b73f-456a-9d97-cdb4500bc7ed/latest-updates-indicated-peasectoa-infection-?forum=WindowsDefenderATPPreview). This thread indicates that MSOnline and the Azure modules are impacted. Lots of good information is collecting here even as I post this.

# Reddit

Reddit is also noticing the issue but does not have much for details at this point.

* [Windows Defender reporting Peasecto.A malware in some Microsoft PSD1 files](https://www.reddit.com/r/PowerShell/comments/7to5dy/windows_defender_reporting_peasectoa_malware_in)
* [System Center Endpoint protection flagging the MSOnline.psd1 file with Peasecto.A trojan](https://www.reddit.com/r/sysadmin/comments/7tnukh/system_center_endpoint_protection_flagging_the)

# Reinstall PowerShellGet and PackageManagement

Because PowerShellGet depends on PackageManagement, issues with PackageManagement can prevent PowerShellGet from working. How do you install modules when Install-Module has issues?

If Windows Defender did clean up files out of the PackageManagement module, you can import the old version of PowerShellGet in a fresh shell to use `Install-Module` again.

    Get-Module PowerShellGet -ListAvailable |
        Where Version -eq 1.0.0.1 |
        Import-Module

    Install-Module PackageManagement -Force
    Install-Module PowerShellGet -Force
