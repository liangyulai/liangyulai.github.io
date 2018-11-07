---
title: Apt can’t see sources? Try changing the User Agent
date: 2016-07-20 14:51:15
tags:
  - Ubuntu
  - APT
---

http://samhassell.com/apt-cant-see-sources-try-changing-the-user-agent/

If you are trying to use apt behind a corporate firewall, try changing the user agent apt is using with wget to load the package lists. By default it uses:

```
User-Agent: Debian APT-HTTP/1.3
```

which isn’t recognised by the filter.

#### Create and edit /etc/apt/apt.conf

```
sudo vi /etc/apt/apt.conf
```

#### Paste the following into the file:

```
Acquire
{
  http::User-Agent "Mozilla/5.0 (Windows NT 5.1; rv:25.0) Gecko/20100101 Firefox/25.0";
};
```

#### Update apt

```
sudo apt-get update
```

Tada.
