+++
date = '2026-01-21T11:54:44-05:00'
draft = false
title = 'Internet Connection'
weight = 5
+++

# Establishing an Internet Connection

While connected to your Raspberry Pi's wireless network, it is useful to also have an **internet connection**. The wireless network broadcast by the Raspberry Pi will act as a **repeater node**, and allow devices connected to it to access the internet.

## Connecting Through the Command Line

An internet connection can be established by first creating a connection for [Network Manager](https://networkmanager.dev). MIT uses what's called [802.1x](https://en.wikipedia.org/wiki/IEEE_802.1X) authentication for establishing connections, which requires both a **username** and **password**. We'll configure this using either the scripted, or manual methods below.

{{< tabs >}}
{{% tab "Scripted" %}}
Use the following command where `wlan0` referes to the network interface being configured (leave this), and `<YOUR-KERBEROS>` refers to your **MIT kerberos** username.

```
wifi wlan0 <YOUR-KERBEROS>
```

{{% /tab %}}
{{% tab "Manual" %}}
For manual configuration, use the following command where `MIT SECURE` is the ssid, or the name of the network we're trying to connect to, and `wlan0` is network interface we're configuring to it.


```
nmcli connection add type wifi con-name "MIT" ifname wlan0 ssid "MIT SECURE" \
    wifi-sec.key-mgmt wpa-eap \
    802-1x.eap peap \
    802-1x.phase2-auth mschapv2 \
    802-1x.identity "YOUR-KERBOROS"
```

The connection can then be **enabled** by executing the following:

```
nmcli --ask deivce connect wlan0
```

{{% /tab %}}
{{< /tabs >}}

