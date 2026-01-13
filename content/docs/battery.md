---
title: "Battery"
weight: 7
draft: true
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# Battery how-to
Guide for inspecting, storing, charging, and using the battery

## Battery components

The battery system is packages with several components for safe battery usage. Here is the list of battery components in your kit:

<center>

| Component | Quantity |
|-|-|
| LiPo battery | 2 |
| LiPo charger | 1 |
| LiPo safety bag | 2 |
| Low voltage monitor | 2 |
| Switch cable | 1 |
<!-- | Fused extension cable | 1 | -->

</center>

### LiPo battery
The LiPo battery for the Pi is a LiPo battery with 3 cells connected in series (3S battery) with 5200mAh. This should keeps the robot fully **running for ~2 hours**, or **idling for ~6 hours**. The battery has an XT60 connector for power output and a JST-XH balancing lead for monitoring individual cells. Read the manufacturer's manual for more information.

<p align="center">
<img src="/images/battery.jpg" width="50%" />
</p>

### LiPo charger
The charger included is a LiPo balancing lead charger. It charges and monitor individual battery cell's voltage through the balancing lead to make sure no cell exceeds the rated voltage of LiPo battery (4.2V). The charger is designed to charge at up to 2000mA per cell and supports 2 or 3 cells battery. It does **NOT** support charging **2 batteries at the same time**. Read the manufacturer's manual for more information.

<p align="center">
<img src="/images/charger.jpg" width="50%" />
</p>

### LiPo safety bag
Two fireproof bags are provided for storing the batteries while not in use.

<p align="center">
<img src="/images/safety_bag.jpg" width="50%" />
</p>

<!-- ## Fused extension cable
For additional protection when the battery is not plugged into the [Raven board](2-Electrical#hardwares), a 30A fused cable is provided for overcurrent protection. The fuse is replacable but let's do our best to never have to replace it.

<p align="center">
<img src="/images/fused_cable.jpg" width="50%" />
</p> -->

### Switch extension cable
In case the robot becomes sentient and goes on a rampage, something is burning, or any other emergency situation, a switch extension cable is provided. The switch will turn off connection to the battery and cut off power supply to the robot.

<p align="center">
<img src="/images/switch_cable.jpg" width="50%" />
</p>

### Low voltage monitor
Low voltage monitors are provided to make sure that the battery cell voltages do not fall below a safety threshold (3.3V) during use. They continuously monitor the voltages through the safety leads and let out a loud a buzz whenever any cell reaches the safety threshold.

<p align="center">
<img src="/images/voltage_monitor.png" width="50%" />
</p>

## Inspection

Lipo batteries can be physically damaged (puncture, bend, etc) or electrically damaged (overcharged, overdischarged, overcurrent). Our battery comes in hard plastic shell to prevent physical damage, and we do everything we can to prevent electrical damage. However, accidents can happen and we need to know how to check for damages.

### Physical inspection

#### Swollen battery
Badly damaged LiPo battery will swell. Check for signs of battery swelling. For reference, the left battery is a normal battery and the right battery is damaged.

<p align="center">
<img src="/images/lipo_swollen.jpg" width="75%" />
</p>

> [!TIP]
> Even though our battery has a hard plastic casing, a swelling battery may cause the shell to warp and no longer sits flat on the table.

> [!CAUTION]
> Swollen battery is extremely dangerous and may burst into flame at anytime! Please inform a MASLAB staff immediately for proper disposal once there is any sign of swelling.

#### Leads
The batteries may be subjected to many rounds of plug-unplugs, scraping, electrical problems, etc. These may cause damages to the insulation and the connectors. Please check the wires for any sign of burns, and exposed conductors. Here is some refence for exposed wire and damaged connector:

<p align="center">
<img src="/images/wire_damage.jpg" width="75%" />
</p>

<p align="center">
<img src="/images/xt60_damage.jpg" width="75%" />
</p>

### Electrical inspection
Cell voltage is a good indicator of battery health. Plug in the [low voltage monitor](#low-voltage-monitor) to check for cell voltages. The cell voltages should read **between 3.3V and 4.2V** for a healthy LiPo battery.

> [!IMPORTANT]
> The low voltage monitor is polarized. Please be careful when installing the balancing lead. The black wire should be connected to the first pin (BBX end) as shown in the previous picture. Below is a reference from older generation of the product.
> <p align="center">
> <img src="/images/voltage_monitor_connection.png" width="50%" />
> </p>

## Storage

Here is a general list of steps for storing batteries. For more information, check battery manufacturer's manual.

1. [Inspect.](#inspection)
2. Store in a battery-safe bag.
3. Place the bag in a dry area at room temperature and away from flammable objects.

> [!IMPORTANT]
> LiPo battery performance degrades when left at fully charged or fully depleted for too long. For storing LiPo battery over long period of time (longer than a week), make sure the cell voltages are around 3.8V. Read the battery manufacturer's safety notes for more information.

> [!TIP]
> With degrading performance at higher voltage, it is recommended to **NOT** charge the battery if you do not plan to use it immediately after. Exception to this is when the battery voltage is too low.

## Charge

Here is a general list of steps for charging the batteries. For more information, check the charger manufacturer's manual.

1. [Inspect.](#inspection)
2. Plug the charger into the wall outlet.
3. Plug the battery balancing lead into `3 CELL` slot of the charger. The plug only goes in one way (see picture).
4. Wait for the charger LED to turn from red 🔴 to blue 🔵. 
5. **Do not leave the battery unattended while charging. Any battery charging without a team member nearby will be confiscated.** 

<p align="center">
<img src="/images/charger_connect.jpg" width="75%" />
</p>

> [!CAUTION]
> This LiPo charger is simple. Too simple for charging EVERY kind of battery. It can destroy the batteries, itself, and burn down your place if misused. Therefore, here is a list of DO-NOTs:
> 1. Do not use it for any battery that is not LiPo. 
> 2. Do not use it to charge two batteries at the same time.
> 3. Do not use it to charge battery with lower than 2000mAh.
> 4. Do not leave it unattended while charging.

## Use
Again, [inspect](#inspection) the battery before use. Then make sure to use the following components.

### Low voltage monitor
Please have the low voltage monitor connected to the battery balancing lead **as long as the battery is in use**. When not in use, feel free to remove the monitor and store the battery in the battery safe bag.

> [!TIP]
> Feel free to tape a low voltage monitor to each battery so you will remember to plug it in during use.

### Fused and switch cables
Please connect the battery only to a switch cables. **DO NOT CONNECT THE BATTERY DIRECTLY TO THE RAVEN BOARD**.

> [!TIP]
> You can have these cables attached to your robot. Highly recommend designing a cut-out on your robot to nicely insert the switch.

### Full battery connection diagram

<p align="center">
<img src="/images/battery_diagram.png" width="100%" />
</p>

### Using Pi with battery
> [!CAUTION]
> The battery will also power the Pi on through Raven board and conflicts with the USB-C power adapter.  
> **_DO NOT USE THE USB-C POWER ADAPTER WHILE THE BATTERY IS PLUGGED IN!_**  
> **_DO NOT PLUG IN THE BATTERY WHILE USING USB-C POWER ADAPTER!_**

Your Raven should be updated [with the latest firmware](4-Software#install-and-update-maslab-software) **BEFORE** using with the battery. With [Raven installed on top of a Pi](2-Electrical#pi-connection):

1. Connect the [power button](2-Electrical#power-button-connection) to Raven.
1. Connect the battery to Raven following the [usage guide](#use)
1. Press the power button momentarily

The Pi should turn on and stay on after the button is released. 

To shutdown the Pi while the battery is plugged in, follow the [normal shutdown procedure](3-Raspberry-Pi#shutting-down).

## Additional resources
MIT EHS provides a general checklist for using Lithium batteries. Feel free to print them out for references. They can be downloaded here: https://ehs.mit.edu/wp-content/uploads/2019/09/Lithium_Battery_Checklist.pdf

## What's next
With the battery, you can start powering your Pi and motors with the full electrical oomph of LiPo. PLEASE be careful with the battery as it can fail catastrophically when misused. Check out [software guide](4-Software) to start using the motors.
