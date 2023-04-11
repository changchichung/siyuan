---
title: remove snap firefox , and install firefox with apt in ubuntu 22.04
slug: remove-snap-firefox-and-install-firefox-with-apt-in-ubuntu-2204-z1mfxgv
url: >-
  /post/remove-snap-firefox-and-install-firefox-with-apt-in-ubuntu-2204-z1mfxgv.html
tags: []
categories: []
lastmod: '2023-04-11 14:07:34'
toc: true
keywords: ''
description: >-
  removesnapfirefoxandinstallfirefoxwithaptinubunturemovethefirefoxsnapsudosnapremovefirefoxaddthe(ubuntu)mozillateamppasudoaddaptrepositoryppa_mozillateamppaalterthefirefoxpackagepriorityechopackage_pin_releaseo=lpppamozillateampinpriority__sudoteeetcaptpr
isCJKLanguage: true
---

# remove snap firefox , and install firefox with apt in ubuntu 22.04

### Remove the Firefox Snap

```
sudo snap remove firefox
```

### Add the (Ubuntu) Mozilla team PPA

```
sudo add-apt-repository ppa:mozillateam/ppa
```

### alter the Firefox package priority

```
echo '
Package: *
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001
' | sudo tee /etc/apt/preferences.d/mozilla-firefox
```

### Firefox upgrades to be installed automatically

```
echo 'Unattended-Upgrade::Allowed-Origins:: "LP-PPA-mozillateam:${distro_codename}";' | sudo tee /etc/apt/apt.conf.d/51unattended-upgrades-firefox
```

### install Firefox via apt

```
sudo apt install firefox
```

‍