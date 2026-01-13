---
title: "Raspberry Pi"
date: 2026-01-08T14:04:32-05:00
weight: 3
keywords:
- wifi
- raspberry pi
- ssh
# bookComments: false
# bookSearchExclude: false
# bookPostThumbnail: thumbnail.*
---

# Raspberry Pi 
The machine given to you by staff is called a Raspberry Pi. For the purposes of this class, your Raspberry Pi comes preinstalled with a slightly customized copy of [Raspberry Pi OS -- Trixie](https://www.raspberrypi.com/news/trixie-the-new-version-of-raspberry-pi-os/).

## Setting up the Pi
The Raspberry Pi 5 requires an active cooler to properly cool the board during heavy computations (i.e. /images processing). To install the cooler, follow the installation guide at: https://datasheets.raspberrypi.com/cooling/raspberry-pi-active-cooler-product-brief.pdf 

<p align="center">
<img src="/images/pi5_cooler.png" width="50%" />
</p>

Along with the Pi you should have received a microSD card in a microSD card case labeled with your team name. You should insert the given microSD card into the microSD card slot found on the underside of the Pi.

<p align="center">
<img src="/images/pi5_sd.png" width="50%" />
</p>

For WiFi connection, your kit should include a new USB to WiFi adapter. Unbox it and plug it into a USB 2.0 slot (black USB slot) on the Pi.

<p align="center">
<img src="/images/tp_link_nano.png" width="30%" />
</p>

For powering the Pi, you should have received a 27W AC-DC converter sporting a usb-C cable. The Pi comes equipped with a single usb-C port, which you should connect this power supply to.

<p align="center">
<img src="/images/pi5_power.png" width="50%" />
</p>

After inserting the microSD card, the WiFi adapter, and connecting the Pi to power, you should see an array of blinking lights and a momentary spinning of the on-board cooler. During this time, your Pi is booting and running its first-time-setup scripts. Once the power indicator (the, more often than not, green blinking light) momentarily turns **red**, the Pi will have rebooted. After rebooting, the power indicator should return to its green-blinking state and is ready for you to hop in.

<p align="center">
<img src="/images/pi5_led.png" width="50%" />
</p>

## Using the Pi
### Pi's Hotspot
After your Pi has completed it's initialization, it should now be broadcasting a wireless network labeled after your team name; e.g. `team1wifi`. Ask the staff for your team's WiFi password and connect to the 

> [!WARNING]
> After connecting to your Pi's wireless network, you will not have access to the "internet" until later on in this guide.

### Connecting over SSH
> [!NOTE]
> Having a basic understanding of navigating a [shell](https://en.wikipedia.org/wiki/Shell_(computing)) through a [command-line interface](https://en.wikipedia.org/wiki/Command-line_interface) (CLI) to interact with a computer without a screen is an extremely useful skill. The shell you will use to connect to your Pi will differ system-to-system. On Apple laptops, this is done through the use of the **zsh**-shell, which can be accessed through spotlight by searching for "terminal". On a Windows machine, this is done through the Command Prompt, or the Windows PowerShell. On Linux devices, this is done through whatever shell you please. It is highly encouraged to your familiarize yourself with the [basics](https://ubuntu.com/tutorials/command-line-for-beginners) before proceeding.
u
To connect to the Pi, we use [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell). SSH is an extremely powerful tool for remotely accessing another computer's shell. This class involves a significant amount of coding, and being able to do so in the absence of a desktop environment is a very valuable skill. SSH allows multiple agents to remotely attach the same machine, at the same time. Most modern operating systems like OS X, Ubuntu, and Windows come with SSH preinstalled. Double check by running `ssh -V` in a shell. If SSH is installed, you should get something similar to this:

```shell
PS C:\Users\MASLAB> ssh -V
OpenSSH_for_Windows_9.5p1, LibreSSL 3.8.2
```

To connect to your Pi using SSH, you need to first be connected to its broadcasted wireless network. Open a shell, or SSH-client / application and enter the following command, where X is your team number:

```shell
ssh teamX@teamXpi.local
```

The format of this is as follows: `ssh user@hostname`; where the `user` is your team name, and the `hostname` is your your team name with the `pi.local` suffix appended. You should keep note of your device's hostname.


You should be shown a field resembling the following and prompted for a password. Ask a staff for the password.

```
teamX@teamXpi.local's password: 
```

There are a variety of tools to make your life easier while working in a shell. Some of the staff's personal favorites are [vim](https://www.vim.org/) or [neovim](https://neovim.io/) in tandem with [tmux](https://github.com/tmux/tmux/wiki). Becoming proficient with these tools can, in many ways, be much more powerful than a traditional desktop IDE.

#### Shell Tool Resources
- [Vim Tutorial](https://openvim.com/)
- [Tmux Tutiorial](https://hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)


### Connecting over RDP
There are times in which accessing a desktop on your Pi will be important. To do so, we use [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol).

#### RDP on OSX/iOS
On an OSX/iOS device, the [Windows App](https://apps.apple.com/us/app/windows-app/id1295203466) application on the app store can be used. In the application, create a new connection by clicking the plus icon, and navigating to the "PC Name" dropdown. In the field labeled `Hostname or IP Address`, enter your team's **hostname**, `teamXpi.local` where X is your team number. Exit the dropdown and save.

To start a connection to your device, click on the saved configuration in the application's "devices" tab. When prompted for a username and password, enter your team's credentials:

```
user: teamX
password: <ask staff>
```

#### RDP on Windows
On a Windows device, you can use the preinstalled "Remote Desktop Connection" app. For "Computer", enter your team's **hostname**, `teamXpi.local` where X is your team number.

<p align="center">
<img src="/images/rdp.png" width="50%" />
</p>

There will be a warning about the computer's certificate. Feel free to select "Don't ask me again for connections to this computer" and click "Yes" to connect.
<p align="center">
<img src="/images/rdp_warning.png" width="50%" />
</p>

Enter username as `teamX` where X is your team number your team's password.
<p align="center">
<img src="/images/rdp_signin.png" width="75%" />
</p>

### Connecting with VSCode
[Visual Studio Code](https://code.visualstudio.com/) (VSCode) is a very popular code editor for many languages, including [Python](https://www.python.org/) which runs the [MASLAB library](4-Software#maslab-software-library).

We would recommend using VSCode to interact with the Pi for the majority of your robot software development. You can download the appropriate version at https://code.visualstudio.com. 

VSCode has an extension to remotely connect to the Pi's and open its folders directly on VSCode. The extension is available here: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh 

Install and follow the [Getting started](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh#getting-started) note with simple SSH host setup. 

### Shutting down
The Pi is a full computer running a full operating system that needs to stop background processes when shutting down. Therefore, you should [run this command in a shell connected to the Pi](#connecting-over-ssh) to gracefully shut it down.

```shell
sudo shutdown 0
```

The `0` in the command indicates that we want to shutdown with 0 wait instead of the default 1 minute wait before shutdown.

Alternatively, you can log in with RDP and use the graphical interface to shutdown.

<p align="center">
<img src="/images/pi_shutdown.png" width="75%" />
</p>


## Connecting the Pi to MIT Wifi
After you've successfully connected to your Pi via either SSH or XRDP, you should now connect your Pi to the internet. This can be done either over SSH or XRDP through the desktop. It is recommended to connect to MIT's wifi using the desktop over XRDP.

<p align="center">
<img src="/images/pi_wifi.png" width="75%" />
</p>

However, this can also be done by using the `raspi-config` tool, navigating to the "System Options" tab, selecting Wireless LAN, and following the prompts.

Once you have connected the Pi to WiFi, your computer should also have access to the Internet through the Pi's [hotspot](#pis-hotspot).

# What's next?
Congratulations! You have set up and familiarized with the Pi. If you haven't already, please checkout the following how-tos to interact with the electronics, battery, and MASLAB software.
* [Electrical how-to](2-Electrical)
* [Software how-to](4-Software)
* [Battery how-to](5-Battery)
