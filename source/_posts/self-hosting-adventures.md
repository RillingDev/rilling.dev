---
title: "Self-Hosting Adventures"
date: 2022-03-05
updated: 2023-09-30
tags:
    - Self-Hosting
    - FOSS
    - Infrastructure
description: "I have been trying to get as independent as possible from online services due to privacy reasons. This article describes my setup and struggles I had with it."
---

For the past few years, I have been trying to get as independent as possible from big, "free" online services (mostly Google stuff) due to privacy reasons. I now primarily use [free open-source software](https://en.wikipedia.org/wiki/Free_and_open-source_software) for these purposes.
This article describes my setup and the struggles I had with it.

<!-- more -->

## Devices Involved

I currently use:

- a desktop computer running GNU/Linux
- an Android smartphone running [LineageOS](https://lineageos.org/)
- a [Raspberry Pi](https://www.raspberrypi.com) running Raspberry Pi OS Lite

In general, a similar setup should be possible with most other kinds of devices.

## File Synchronization

[Syncthing](https://syncthing.net/) is a great tool that can be used to synchronize files across several devices. With it running on all my devices, I can access my photos, documents and password databases anywhere.

## Password Management

[KeePass](https://keepass.info/) or the cross-platform client [KeePassXC](https://keepassxc.org/), as well as the mobile app [Keepass2Android](https://github.com/PhilippC/keepass2android), allows for easy management of credentials.
If combined with Syncthing, you can do so from any of your devices. Note that in rare cases the password files on different devices can conflict with each other during synchronization. In this case, the "Merge from database" feature in KeePassXC is useful.

## Mailing

Self-hosting email infrastructure is tricky. If you are unlucky, you have to deal with email providers rejecting mails from your mail server for arbitrary reasons, even if your mail server and domain are properly set up. Having to deal with the respective email providers' process to get your server allowlisted can be highly frustrating.
Because of this, I chose to not self-host my email infrastructure and instead use [Proton Mail](https://proton.me/mail) (proprietary; I am not affiliated with them, but it has worked well so far).
I still use my own domain for the email addresses to have control over them.

## Contacts, Calendars & Tasks

Previously I used Google's services to synchronize (and backup) contacts and calendar entries from my phone.
As a replacement, I picked [Radicale](https://radicale.org), a tool that supports both [CalDAV](https://en.wikipedia.org/wiki/CalDAV) (for calendars/tasks) and [CardDAV](https://en.wikipedia.org/wiki/CardDAV) (for contacts). The Radicale server runs on the Raspberry Pi I have set up in my home network.
To use this server with my smartphone, [DAVx5](https://www.davx5.com/) is used. Once it is configured to use the Radicale server, my contacts, tasks and calendar entries are available on my smartphone.
I use [Thunderbird](https://www.thunderbird.net) on my desktop computer, which supports CalDAV and CardDAV natively.

## Note-Taking

As Syncthing is already in use, I use plain text or markdown files to take notes, which are then shared across all devices. On my smartphone, I use the app [Markor](https://f-droid.org/en/packages/net.gsantner.markor/) to edit my notes. On my desktop computer, I use [Kate](https://apps.kde.org/kate/) for editing.

## Backups

[Borg](https://www.borgbackup.org/) is a great tool for backups. I use it via [borgmatic](https://torsion.org/borgmatic/), which allows me to create configuration files for different backup types, which are then stored both locally and on a [Hetzner Storage Box](https://www.hetzner.com/storage/storage-box) (proprietary; I am not affiliated with them, but I like their service). Because the Borg backups are encrypted, storing the backup "in the cloud" is acceptable to me.
