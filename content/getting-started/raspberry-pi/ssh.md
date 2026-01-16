---
title: "SSH / MOSH"
weight: 3
keywords:
- SSH
- Mosh

# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# SSH / MOSH

Secure Shell, abbreviated as **SSH**, is an incredibly powerful tool for remotely accessing a networked device. In this class, we will primary use SSH, or its mobile-alternative **MOSH**, to interface with the Raspberry Pi.

Before interfacing with the Raspberry Pi, there are a few steps to setup its connectivity.

> [!TIP]
> If you're experiencing connectivity issues with your Raspberry Pi (stutters, freezing, etc.), it is recommended to use **MOSH** over **SSH**. 
> 
> **MOSH** is often more stable under intermittent connectivity and, in tandem with **tmux**, can provide a robust working environment.

## Hopping on the same LAN

In order to communicate with your Raspberry Pi, we need to connect to the same Local Area Network or **LAN**, for short.

Assuming your Raspberry Pi has completed it's initialization, it should now be broadcasting a wireless network labeled after your team name; e.g. `team11wifi`. The default password to this network is `balsam` -- that is, "maslab" backwards.

Connect to this network and now both you and your Raspberry Pi will be on the same **LAN**.

> [!WARNING]
> After connecting to your Pi's wireless network, you will not have access to the "internet" until later on in this guide.

## Interfacing with the Raspberry Pi

To access the Raspberry Pi, we utilize **SSH** or **MOSH**. First, make sure you have a valid ssh/mosh client installed on your device.

{{< tabs >}}
{{% tab "MacOS" %}} Open up the "Terminal" application on your **OSX** device; this can found by searching for "Terminal" in the spotlight search or through the applications menu. 

Verify **SSH**/**MOSH** is installedby typing `ssh -V` into the application. If not installed, install **SSH** and **MOSH** by executing `brew install openssh mosh` in the Terminal application. {{% /tab %}}
{{% tab "Windows" %}} Either the windows "Command Prompt" or "Powershell" can be used for **SSH**/**MOSH**.

Verify **SSH**/**MOSH** is installedby typing `ssh -V` into the application. If not installed, install **SSH** by executing `winget install Microsoft.OpenSSH`. Optionally, install **MOSH** by adding the [chrome extension](https://chromewebstore.google.com/detail/mosh/ooiklbnjmhbcgemelgfhaeaocllobloj?pli=1). {{% /tab %}}
{{% tab "Linux" %}}
Verify SSH/MOSH is installed by typing `ssh -V` into the terminal. If not installed, install **SSH** and **MOSH** by executing `apt/apk/pacman install openssh mosh` varying on your chosen linux distribution.
{{% /tab %}}
{{% tab "iOS/Android" %}}
Any number of **SSH**/**MOSH** clients can be installed through your devices application manager. On both **iOS** and **Android**, [Termius](https://termius.com/index.html) is the recommended **SSH** application.
{{% /tab %}}
{{< /tabs >}}

Connect to the Raspberry Pi using either **SSH** or **MOSH** by executing `ssh <user>@<hostname>`, where the `<user>` is your assigned team name and number, and the `<hostname>` is your assigned team name and number with "pi.local" suffixed; i.e. `ssh team11@team11pi.local`.

{{< asciinema
    cast="/casts/ssh.cast"
    loop=true
    autoplay=true
    cols=90
    rows=8
    speed=2 >}}

## Connecting the Raspberry Pi to MIT WiFi

```
nmcli connection add type wifi con-name "MIT SECURE" ifname wlan0 ssid "MIT SECURE" \
    wifi-sec.key-mgmt wpa-eap \
    802-1x.eap peap \
    802-1x.phase2-auth mschapv2 \
    802-1x.identity "your_username" \
    802-1x.password "your_password"
```

```
sudo nmcli device wifi hotspot ssid <hotspot name> password <hotspot password> ifname wlan0
```
