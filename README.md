# TelnetD for NS33J

A lightweight Telnet server for NEXTSTEP 3.3J running on VirtualBox.

## Overview

When working with NEXTSTEP as a guest OS on VirtualBox (running on Linux or macOS as the host), you may occasionally want to log into external servers remotely.

Unfortunately, SSH is not available on NEXTSTEP, so this project provides a simple Telnet server on the host OS. You can first log into this local server from NEXTSTEP, and then connect to external services from there.

Since UTF-8 is the de facto standard character encoding in most environments, this server includes built-in conversion between UTF-8 and the EUC-JP encoding used by NEXTSTEP 3.3J.

> ⚠️ This server is intended for use **within the same machine**.\
> There is **no authentication mechanism** implemented.\
> Be sure to properly configure your **firewall or network settings** to prevent external access.\
> Use at your own risk.

---

## Setup

Edit the following line near the top of `telnetd.c`:

```c
const char *allowed_ips[] = { "127.0.0.1", "192.160.1.2", NULL };
```

Add the IP address of your guest OS to this list.\
(If you're using NAT networking, the guest's IP is the same as the host from the guest's point of view.)

---

## Build

```sh
$ cc -o telnetd telnetd.c
```

If your environment requires it, add the iconv library:

```sh
$ cc -o telnetd telnetd.c -liconv
```

---

## Run the Server

With character encoding conversion (UTF-8 <--> EUC-JP):

```sh
$ ./telnetd -j
```

Without character encoding conversion:

```sh
$ ./telnetd
```

---

## Connect from NEXTSTEP

On NEXTSTEP, use:

```sh
$ telnet <host-ip-address> 2323
```

After connecting, to avoid display glitches, set the terminal type:

```sh
$ TERM=vt100
```

---

## License

This code is released into the public domain. You may use, modify, and distribute it freely without restriction.

The author assumes no responsibility or liability for any damage, loss, or legal issues arising from the use of this software. Use it entirely at your own risk.

