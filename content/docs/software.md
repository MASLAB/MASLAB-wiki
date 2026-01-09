---
title: "Software"
weight: 7
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# Software how-to
Guide and general tips on using MASLAB software and other software to interact with sensors and actuators on the robot. Checkout [electrical how-to](2-Electrical#connecting) to learn how to connect the hardwares.

## Prerequisites
1. Raven board [fully soldered](2-Electrical#soldering) and [installed onto the Pi](2-Electrical#pi-connection). 
1. Raspberry Pi 5 [fully set up](3-Raspberry-Pi#setting-up-the-pi) and [connected to internet](3-Raspberry-Pi#connecting-the-pi-to-mit-wifi).

## Install and update MASLAB software
> [!CAUTION]
> **DO NOT USE THE BATTERY WHILE UPDATING MASLAB SOFTWARE. POWER THE SYSTEM WITH THE USB-C POWER ADAPTER INSTEAD.** 
> The power from the battery to the Pi is managed by Raven such that the update process will disturb its operation and cause the entire system to turn off.

Raven board is completely new! That also means it comes with no firmware installed. Therefore, we need to deploy the firmware by [running this command](3-Raspberry-Pi#using-the-pi) on the Pi:

```shell
maslab-update
```

If everything goes well, you should see something similar to this in your shell:
<p align="center">
<img src="/images/maslab_update.png" width="75%" />
</p>


This command will grab the latest version of the Raven firmware and deploy it on the Raven, so make sure Raven is installed on the Pi before running this command. It will also install the latest version of our software library to use Raven.

> [!TIP]
> MASLAB will always be a work in progress. We may release updated versions of Raven firmware and software library as we (and you) discover bugs and improve our software. Therefore, remember to run this command again when we announce software updates.

## Developing with Python and VSCode
If you are using VSCode for developing and [connecting to the Pi](3-Raspberry-Pi#connecting-with-vscode), it is highly recommended to also install VSCode's [Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

If you are not familiar with Python, please follow VSCode's tutorial on how to develop with Python: https://code.visualstudio.com/docs/python/python-tutorial.

And of course please feel free to ask course staffs any question!

## MASLAB software library
MASLAB staffs maintain a Python library that supports using the motors and servos on the Raven board (`raven`).

> [!IMPORTANT]
> Since these library is communicating with a hardware, no two instances may run at the same time. **DO NOT** have two different Python scripts trying to use Raven.

The library is available at https://github.com/MASLAB/maslab-lib.

### Import Raven
```Python
from raven import Raven

raven_board = Raven()
```

## DC Motors
> [!IMPORTANT]
> The motor is powered by the battery such that your Pi + Raven setup needs to be [powered by the battery](5-Battery#using-pi-with-battery) to run the motor.
> The provided motor is a **POWERFUL** motor. Please have it secured to your table / robot before testing. Test at low speed / torque before going full power!

DC motors may come with encoders to estimate how many rotation has the motor rotated. To increase the resolution of the encoder, the encoder spins with some gear ratio with respect to the motor, which also has another gear ratio. For example, the provided motor is a 50:1 motor with an encoder that does 64 counts per revolution (also called pulse per revolution). This means the encoder will count 50 * 64 = 3200 counts per motor rotation.

Raven supports up to [5 DC motors with encoders](2-Electrical#motor-connection). 

### Encoder
With Raven, you can get the encoder values. You can also set them to a new initial value. Here is an example of how to use it:

```Python
raven_board.set_motor_encoder(Raven.MotorChannel.CH1, 0) # Set encoder count for motor 1 to zero
print(raven_board.get_motor_encoder(Raven.MotorChannel.CH1)) # Print encoder count = "0"
```

> [!IMPORTANT] 
> Encoder count should matches with the motor direction. To test, run the motor with direct mode (see [direct mode](#direct-mode)) and read out encoder reading. When motor is going forward (`reverse=False`), encoder counts should go up. If not, swap the C1 and C2 wires. See [motor connection](2-Electrical#motor-connection) for more information.

### Motor
There are 3 ways to drive the motors:
* Direct - Motor moves according to a torque and speed factor. We recommend driving in direct mode first to get things started.
* Position (requires encoder) - Motor moves to an encoder position with PID control
* Velocity (requires encoder) - Motor moves with a set encoder velocity using PID control
* Disabled - Turn off the motor

To use the motors, make sure you have [imported Raven](#import-raven). Then follow the following examples for each mode.

> [!TIP]
> Once motor mode and PID values are set, you can set motor drive values without having to set the mode and PID values again.

#### Direct mode
In direct mode, you get to set the torque factor as 0% to 100% of available torque. You also get to set the speed as 0% to 100% of max speed and a direction of rotation. These conditions are subjected to the battery's voltage. Do this for most reliable actuation or custom controls. Also useful as a part of mechanical designs.

```Python
raven_board.set_motor_mode(Raven.MotorChannel.CH1, Raven.MotorMode.DIRECT) # Set motor mode to DIRECT

# Speed controlled:
raven_board.set_motor_torque_factor(Raven.MotorChannel.CH1, 100) # Let the motor use all the torque to get to speed factor
raven_board.set_motor_speed_factor(Raven.MotorChannel.CH1, 10, reverse=True) # Spin at 10% max speed in reverse

# Torque controlled:
raven_board.set_motor_speed_factor(Raven.MotorChannel.CH1, 100) # Make motor try to run at max speed forward
raven_board.set_motor_torque_factor(Raven.MotorChannel.CH1, 10) # Let it use up to 10% available torque
```

#### Controlled mode
> [!IMPORTANT]
> The control loop for Raven motor runs at 1kHz (dt = 0.001). This will be important to playing with PID values.

> [!CAUTION]
> The PID values provided are only as examples. They are untested and may not work for your motors. Here is some tip to tune the values:
> 1. Start with some P and adjust until your motor starts going to desired target quickly but may not reach / little overshoot.
> 2. Include I and adjust until your motor gets up to target set point quickly and settle to correct target point with some overshoot.
> 3. Include D and adjust until your motor get to set point still quickly and correctly but does not overshoot.
>
> Often time you only needs PI or PD to control your motors. Here is also a great video to demonstrate the effects of PID: https://www.youtube.com/watch?v=fusr9eTceEo

> [!TIP]
> Check control theory lecture notes to refresh about PID control: https://maslab.mit.edu/2025/lectures

For controlled modes, you will need to provide a current limit for the motor effort. Maximum allowed current is 6.4 amps.

```python
raven_board.set_motor_max_current(Raven.MotorChannel.CH1, 5) # Set motor current to 5 amps
```

##### Position controlled
In position controlled mode, you get to set the PID value for the controller and a target in encoder counts.

```Python
raven_board.set_motor_encoder(Raven.MotorChannel.CH1, 0) # Reset encoder
raven_board.set_motor_max_current(Raven.MotorChannel.CH1, 5) # Set motor current to 5 amps
raven_board.set_motor_mode(Raven.MotorChannel.CH1, Raven.MotorMode.POSITION) # Set motor mode to POSITION
raven_board.set_motor_pid(Raven.MotorChannel.CH1, p_gain = 100, i_gain = 0, d_gain = 0) # Set PID values

# Make the motor spin until 4400 counts (10 rev of wheel motor)
raven_board.set_motor_target(Raven.MotorChannel.CH1, 4400)
```

##### Velocity controlled
In velocity controlled mode, you also get to set PID value and target in encoder counts per second.

```Python
raven_board.set_motor_encoder(Raven.MotorChannel.CH1, 0) # Reset encoder
raven_board.set_motor_max_current(Raven.MotorChannel.CH1, 5) # Set motor current to 5 amps
raven_board.set_motor_mode(Raven.MotorChannel.CH1, Raven.MotorMode.VELOCITY) # Set motor mode to POSITION
raven_board.set_motor_pid(Raven.MotorChannel.CH1, p_gain = 10, i_gain = 0, d_gain = 0) # Set PID values

# Make the motor spin at -4400 counts/second (-10 rev/sec of wheel motor)
raven_board.set_motor_target(Raven.MotorChannel.CH1, -4400)
```

## Servos
Servos are position controlled motors that can move from -90 degree to 90 degree. Their signal is based on a [timed pulse](https://en.wikipedia.org/wiki/Servo_control). Typically, 1000us means -90 degree and 2000us means 90 degree. Some servo may have different values.

<p align="center">
<img src="/images/servo_timing.png" width="75%" />
</p>

Raven board supports up to [4 servos](2-Electrical#servo-connection). For each servo, you get to set the position in degree. Optionally, you can set the minimum and maximum pulse microsecond as `min_us` and `max_us` for you appropriate motor. Otherwise, they are defaulted to `min_us=1000` and `max_us=2000`. Here is an example of how to use them once you have [imported Raven](#import-raven).

```Python
# Set the servo 1 to -75 degrees with custom pulse microseconds
raven_board.set_servo_position(Raven.ServoChannel.CH1, -75, min_us=500, max_us=2500)
```

## What's next?
Now that you have the software and firmware to run Raven. **AFTER** the battery lecture and reviewing the [battery guide](5-Battery). Feel free to power your Pi [with the battery](5-Battery#using-pi-with-battery) and see if you can make your motors and servos move!

Once you are familiar with using the Pi and the MASLAB library to interact with Raven, you can start thinking about writing and testing out your robot software. To keep your software development progress organized, we will be using [Git version control system](https://en.wikipedia.org/wiki/Git). Checkout [Git guide](6-Git) to "git" started.
