---
title: "Self-Hosting Adventures"
date: 2022-03-05
updated: 2022-03-05
tags:
    - Self-Hosting
    - FOSS
---

The past few years I have been trying to get as independent as possible from big, "free" online services (mostly Google stuff) due to privacy reasons. Here I will write my setup, and the struggles I had with it.

<!-- more -->

## Devices Involved

I currently use:

-   1 desktop computer running GNU/Linux
-   1 Android smartphone running [LineageOS](https://lineageos.org/)
-   1 [Raspberry Pi](https://www.raspberrypi.com) running Raspberry Pi OS Lite

In general, a similar setup should be possible with most other kinds of devices.

## File Synchronization

[Syncthing](https://syncthing.net/) is a great tool that can be used to synchronize files across several devices. With it running on all my devices, I can access my photos, documents, and password databases anywhere.

## Password Management

[KeePass](https://keepass.info/) or the cross-platform client [KeePassXC](https://keepassxc.org/), as well as the mobile app [Keepass2Android](https://github.com/PhilippC/keepass2android), allow for easy management of credentials.
If combined with Syncthing, you can do so from any of your devices. It should be noted that in rare cases the password files on different devices can conflict with each other during synchronization. In this case, the "Merge from database" feature in KeePassXC is useful.

## Mailing

Here I have to cheat a bit: Because the mail server has to be reachable from the internet to be useful, I chose not to fully self-host it. Instead, any web-hosting service (that you trust to not read your data) where you can run and administer a GNU/Linux OS should suffice. In addition, a domain name to send/retrieve mail for has to be owned.

[Postfix](https://www.postfix.org/) is a great FOSS mail server that is quite powerful. [Dovecot](https://www.dovecot.org/) is used to retrieve mails via IMAP. A guide on how to configure those two to integrate can be found [in the Dovecot documentation](https://doc.dovecot.org/configuration_manual/howto/postfix_and_dovecot_sasl/).

To make sure mail that is sent from your server is accepted by the recipients' server, quite a few other steps are needed:

-   Set up [SPF](https://en.wikipedia.org/wiki/Sender_Policy_Framework) in your DNS records.
-   Set up [DKIM](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail) by installing [OpenDKIM](http://www.opendkim.org/), [integrating it into Postfix](https://easydmarc.com/blog/how-to-configure-dkim-opendkim-with-postfix/), and adding the new DNS records.
-   Set up [DMARC](https://easydmarc.com/blog/how-to-configure-dkim-opendkim-with-postfix/).
-   Check for all other kinds of issues that may arise. [MXToolbox](https://mxtoolbox.com/SuperTool.aspx) can test most of it, but only time will tell if everything works.

Mailing has been by far the most frustrating part of this project, because of how many things intertwine, and because of how hard it is to test if everything works.

## Contacts, Calendars & Tasks

Previously I used Google's services to synchronize (and backup) contacts and calendar entries from my phone.
As a replacement, I picked [Radicale](https://radicale.org), which is a FOSS tool that supports both [CalDAV](https://en.wikipedia.org/wiki/CalDAV) (for calendars/tasks) and [CardDAV](https://en.wikipedia.org/wiki/CardDAV) (for contacts). The Radicale server runs on the Raspberry Pi I have set up in my home network.
To use this server with my smartphone, [DAVx5](https://www.davx5.com/) is used. Once it is configured to use the Radicale server, my contacts, tasks, and calendar entries are accessible.
I use [Thunderbird](https://www.thunderbird.net) on my desktop computer, which supports CalDAV and CardDAV natively.

## Note-Taking

As Syncthing is already in use, I use plain text or markdown files to take notes, which are then shared across all devices. On my smartphone, I use the app [Markor](https://f-droid.org/en/packages/net.gsantner.markor/), while any text editor suffices on my desktop computer.
