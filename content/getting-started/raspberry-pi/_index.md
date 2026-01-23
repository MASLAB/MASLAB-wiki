---
title: "Raspberry Pi"
weight: 3
keywords:
- wifi
- raspberry pi
- ssh
- tmux
- vim
- neovim
# bookComments: false
# bookSearchExclude: false
# bookPostThumbnail: thumbnail.*
bookCollapseSection: false
---

# Raspberry Pi 
A [Raspberry Pi](https://www.raspberrypi.com) is a powerful **single-board-computer (SBC)** popular among hobbiests and researchers. For the duration of this class, you'll be using a **Raspberry Pi 5**, running a slightly modified fork of  [Debian Trixie](https://www.raspberrypi.com/news/trixie-the-new-version-of-raspberry-pi-os/).
                    
{{< figure src="/images/rpi5.jpg" >}}

## Setting up the Pi
In this section there are a number of **links** which point to external guides. When prompted, follow these for specific setup information.

### Cooling
The Raspberry Pi 5 requires an **active cooler** to properly cool the board during heavy computations. To install the cooler, follow the [installation guide](https://datasheets.raspberrypi.com/cooling/raspberry-pi-active-cooler-product-brief.pdf).

{{< figure src="/images/cooler.jpg" >}}

### MicroSD Card
Along with the Pi is a **microSD** card in a case labeled with your **team name** and **number**. Insert the microSD card into the microSD card slot found on the **underside** of the Pi.

{{< figure src="/images/pi5_sd.png" >}}

### Wifi Antenna
In this class you'll be connecting to your **Raspberry Pi** over a broadcasted wireless network. Your kit includes a **TP Link** wireless adapter. Unbox and plug this into your Raspberry Pi.

{{< figure src="/images/tp_link_nano.png" >}}

### Power Adapter
For powering the Pi, use the **27W AC-DC** converter sporting a usb-C cable supplied in your kit. The Pi comes equipped with a single **usb-C port**, which you should connect this power supply to.

{{< figure src="/images/pi5_power.png" >}}


## First Boot
After inserting the **microSD card**, the **WiFi adapter**, and connecting the Pi to **power**, you should see an array of blinking lights and a momentary spinning of the on-board cooler. During this time, your Pi is booting and running its first-time-setup scripts. Once the power indicator (the, more often than not, green blinking light) momentarily turns **red**, the Pi will have rebooted. After rebooting, the power indicator should return to its green-blinking state and is ready for you to hop in.

{{< figure src="/images/pi5_led.png" >}}

### Connecting to WiFi
After your Pi has completed it's initialization, it should now be **broadcasting a wireless network** labeled after your team name; e.g. `team05`. Connect to this wireless network with the password `balsam<team-number>` based on your team name; e.g. `balsam05` for **team 5**.

> [!WARNING]
> After connecting to your Pi's wireless network, you will not have access to the "internet" until later on in this guide. It is recommended to open the following **two** sections ([SSH](./ssh), [Internet Connection](./wifi)) in separate tabs before proceeding.

Once connected, proceed to the following section to {{< button href="./ssh" >}}SSH{{< /button >}} into your Raspberry Pi.


<!---->
<!-- #### RDP on OSX/iOS -->
<!-- On an OSX/iOS device, the [Windows App](https://apps.apple.com/us/app/windows-app/id1295203466) application on the app store can be used. In the application, create a new connection by clicking the plus icon, and navigating to the "PC Name" dropdown. In the field labeled `Hostname or IP Address`, enter your team's **hostname**, `teamXpi.local` where X is your team number. Exit the dropdown and save. -->
<!---->
<!-- To start a connection to your device, click on the saved configuration in the application's "devices" tab. When prompted for a username and password, enter your team's credentials: -->
<!---->
<!-- ``` -->
<!-- user: teamX -->
<!-- password: <ask staff> -->
<!-- ``` -->
<!---->
<!-- #### RDP on Windows -->
<!-- On a Windows device, you can use the preinstalled "Remote Desktop Connection" app. For "Computer", enter your team's **hostname**, `teamXpi.local` where X is your team number. -->
<!---->
<!-- {{< figure src="/images/rdp.png" width="50%" >}} -->
<!---->
<!-- There will be a warning about the computer's certificate. Feel free to select "Don't ask me again for connections to this computer" and click "Yes" to connect. -->
<!---->
<!-- {{< figure src="/images/rdp_warning.png" width="50%" >}} -->
<!---->
<!-- Enter username as `teamX` where X is your team number your team's password. -->
<!---->
<!-- {{< figure src="/images/rdp_signin.png" width="75%" >}} -->
<!---->
<!-- ### Connecting with VSCode -->
<!-- [Visual Studio Code](https://code.visualstudio.com/) (VSCode) is a very popular code editor for many languages, including [Python](https://www.python.org/) which runs the [MASLAB library](../software#maslab-software-library). -->
<!---->
<!-- We would recommend using VSCode to interact with the Pi for the majority of your robot software development. You can download the appropriate version at https://code.visualstudio.com.  -->
<!---->
<!-- VSCode has an extension to remotely connect to the Pi's and open its folders directly on VSCode. The extension is available here: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh  -->
<!---->
<!-- Install and follow the [Getting started](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh#getting-started) note with simple SSH host setup.  -->
<!---->
<!-- ### Shutting down -->
<!-- The Pi is a full computer running a full operating system that needs to stop background processes when shutting down. Therefore, you should [run this command in a shell connected to the Pi](#connecting-over-ssh) to gracefully shut it down. -->
<!---->
<!-- ```shell -->
<!-- sudo shutdown 0 -->
<!-- ``` -->
<!---->
<!-- The `0` in the command indicates that we want to shutdown with 0 wait instead of the default 1 minute wait before shutdown. -->
<!---->
<!-- Alternatively, you can log in with RDP and use the graphical interface to shutdown. -->
<!---->
<!-- {{< figure src="/images/pi_shutdown.png" width="75%" >}} -->
<!---->
<!-- ## Connecting the Pi to MIT Wifi -->
<!-- After you've successfully connected to your Pi via either SSH or XRDP, you should now connect your Pi to the internet. This can be done either over SSH or XRDP through the desktop. It is recommended to connect to MIT's wifi using the desktop over XRDP. -->
<!---->
<!-- {{< figure src="/images/pi_wifi.png" width="75%" >}} -->
<!---->
<!-- However, this can also be done by using the `raspi-config` tool, navigating to the "System Options" tab, selecting Wireless LAN, and following the prompts. -->
<!---->
<!-- Once you have connected the Pi to WiFi, your computer should also have access to the Internet through the Pi's [hotspot](#pis-hotspot). -->
<!---->
<!-- ## What's next? -->
<!-- Congratulations! You have set up and familiarized with the Pi. If you haven't already, please checkout the following how-tos to interact with the electronics, battery, and MASLAB software. -->
<!-- * [Electrical how-to](../electrical) -->
<!-- * [Software how-to](../software) -->
<!-- * [Battery how-to](../battery) -->
