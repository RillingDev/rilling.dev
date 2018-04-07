---
title: 'How to remove EVERYTHING from Windows 10 you dont need'
published: true
visible: true
date: '11-11-2015 15:14'
icon: ban
taxonomy:
    category:
        - Windows
    tag:
        - Windows
        - OS
        - Privacy
---

When I first installed Windows 10 on my PC I felt like a lot of the
pre-installed software was rather useless, and so I started my Journey
to clean up my Windows and save disk space and improve performance by
remove everything I felt like I don't need.

## Pre-installed Apps

Windows 10 comes with quite a lot of “Apps” like the Music app, which -
in my opinion - are quite bad, so I prefer to use the software of my
choice here. If you want to remove all those Apps, you need to open the
Windows PowerShell which should already be installed on your system.

After you opened the PowerShell, paste the following:

```powershell
Get-AppxPackage -Name *WindowsCamera* | Remove-AppxPackage
Get-AppxPackage -Name *ZuneMusic* | Remove-AppxPackage
Get-AppxPackage -Name *WindowsMaps* | Remove-AppxPackage
Get-AppxPackage -Name *MicrosoftSolitaireCollection* | Remove-AppxPackage
Get-AppxPackage -Name *BingFinance* | Remove-AppxPackage
Get-AppxPackage -Name *ZuneVideo* | Remove-AppxPackage
Get-AppxPackage -Name *BingNews* | Remove-AppxPackage
Get-AppxPackage -Name *WindowsPhone* | Remove-AppxPackage
Get-AppxPackage -Name *Windows.Photos* | Remove-AppxPackage
Get-AppxPackage -Name *BingSports* | Remove-AppxPackage
Get-AppxPackage -Name *XboxApp* | Remove-AppxPackage
Get-AppxPackage -Name *BingWeather* | Remove-AppxPackage
Get-AppxPackage -Name *WindowsSoundRecorder* | Remove-AppxPackage
Get-AppxPackage -Name *3DBuilder* | Remove-AppxPackage
Get-AppxPackage -Name *SkypeApp* | Remove-AppxPackage
Get-AppxPackage -Name *MicrosoftOfficeHub* | Remove-AppxPackage
```

If you want to keep one of those Apps just remove the line before running the code.

## Cortana, OneDrive & every other privacy risk

One of the biggest additions in Windows 10 is Cortana, a voice-based
assistant. If you're like me and don't like using this type of feature
(and dislike the idea of giving Microsoft your voice data), you can
safely remove Cortana. The same goes for OneDrive, Microsoft's Cloud
service that is integrated into Windows 10. There are some more tools
that are potential risks like the “Windows Update Sharing” which i
recommend to disable.

It’s not really possible to directly disable those in Windows, but the
nice Freeware [Windows Tweaker 4] (Which can do a lot of other cool
stuff) helps with that.

Note: your antivirus might detect Windows Tweaker as trojan because it
modifies Windows files. I used the software for quite a while now and
never had issues, so I’m pretty sure its safe.

[Windows Tweaker 4]: http://www.thewindowsclub.com/ultimate-windows-tweaker-4-windows-10

Once downloaded, run the .exe and go to "Security&Privacy". In the first tab, under "Windows", you can disable OneDrive and the Windows Store. In the "Privacy" tab you can disable Cortana and everything else that might try to spy on you, I disabled everything listened there and haven't had issues with it.
