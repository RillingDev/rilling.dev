---
title: "Self-Hosting Adventures"
date: 2022-02-19
updated: 2022-02-19
tags:
    - Self-Hosting
    - CalDAV
    - CardDAV
---

The past few years I have been trying to get as independent as possible from big, "free" online services (mostly Google stuff) due to privacy reasons. Here I will write down my setup and struggles I had with it.

<!-- more -->

# Devices Involved

I currently use:

- 1 desktop computer running GNU/Linux
- 1 Android smartphone running [LineageOS](https://lineageos.org/)
- 1 [Raspberry Pi](https://www.raspberrypi.com) running Raspberry Pi OS Lite

In general, a similar setup should be possible with most other kinds of devices.

# File Synchronization

# Contacts & Calendars

Previously I used Google's services to synchronize (and backup up) contacts and calendar entries from my phone.
As a replacement, I picked [Radicale](https://radicale.org), which is a FOSS server that supports both CalDAV (calendars/tasks) and CardDAV (contacts). The Radicale server runs on the Raspberry Pi I have set up in my home network. The network configuration is set up to only allow access from my home network, as synchronization is not really a priority if I am not home and thus not connected to said network.
In order to synchronize this server with my (Android) smartphone, [DAVx5](https://www.davx5.com/) is used. Once it is configured to use the Radicale server, my contacts and calendar entries are shared between the two devices.
I use [Thunderbird](https://www.thunderbird.net) on my desktop computer which supports CalDAV and CardDAV natively to complete the contacts and calendar self-hosting setup.

# Mailing
