---
title: "Software"
weight: 4
keywords:
- library
- sensor
- motor
- servo
- raven
# draft: true
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
Guide and general tips on using MASLAB software and other software to interact with sensors and actuators on the robot. Checkout [electrical how-to](../electrical#connecting) to learn how to connect the hardwares.

## Prerequisites
1. Raven board [fully soldered](../electrical#soldering) and [installed onto the Pi](../electrical#pi-connection). 
1. Raspberry Pi 5 [fully set up](../raspberry-pi#setting-up-the-pi) and [connected to internet](../raspberry-pi#connecting-the-pi-to-mit-wifi).

## Install and update MASLAB software
> [!CAUTION]
> **DO NOT USE THE BATTERY WHILE UPDATING MASLAB SOFTWARE. POWER THE SYSTEM WITH THE USB-C POWER ADAPTER INSTEAD.** 
> The power from the battery to the Pi is managed by Raven such that the update process will disturb its operation and cause the entire system to turn off.

Raven board is completely new! That also means it comes with no firmware installed. Therefore, we need to deploy the firmware by [running this command](../raspberry-pi#using-the-pi) on the Pi:

```shell
maslab-update
```

If everything goes well, you should see something similar to this in your shell:

{{< figure src="/images/maslab_update.png" width="75%" >}}

This command will grab the latest version of the Raven firmware and deploy it on the Raven, so make sure Raven is installed on the Pi before running this command. It will also install the latest version of our software library to use Raven.

> [!TIP]
> MASLAB will always be a work in progress. We may release updated versions of Raven firmware and software library as we (and you) discover bugs and improve our software. Therefore, remember to run this command again when we announce software updates.

## Developing with Python and VSCode
If you are using VSCode for developing and [connecting to the Pi](../raspberry-pi#connecting-with-vscode), it is highly recommended to also install VSCode's [Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

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
> The motor is powered by the battery such that your Pi + Raven setup needs to be [powered by the battery](../battery#using-pi-with-battery) to run the motor.
> The provided motor is a **POWERFUL** motor. Please have it secured to your table / robot before testing. Test at low speed / torque before going full power!

DC motors may come with encoders to estimate how many rotation has the motor rotated. To increase the resolution of the encoder, the encoder spins with some gear ratio with respect to the motor, which also has another gear ratio. For example, the provided motor is a 50:1 motor with an encoder that does 64 counts per revolution (also called pulse per revolution). This means the encoder will count 50 * 64 = 3200 counts per motor rotation.

Raven supports up to [5 DC motors with encoders](../electrical#motor-connection). 

### Encoder
With Raven, you can get the encoder values. You can also set them to a new initial value. Here is an example of how to use it:

```Python
raven_board.set_motor_encoder(Raven.MotorChannel.CH1, 0) # Set encoder count for motor 1 to zero
print(raven_board.get_motor_encoder(Raven.MotorChannel.CH1)) # Print encoder count = "0"
```

> [!IMPORTANT] 
> Encoder count should matches with the motor direction. To test, run the motor with direct mode (see [direct mode](#direct-mode)) and read out encoder reading. When motor is going forward (`reverse=False`), encoder counts should go up. If not, swap the C1 and C2 wires. See [motor connection](../electrical#motor-connection) for more information.

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

```python
raven_board.set_motor_encoder(Raven.MotorChannel.CH1, 0) # Reset encoder
raven_board.set_motor_max_current(Raven.MotorChannel.CH1, 5) # Set motor current to 5 amps
raven_board.set_motor_mode(Raven.MotorChannel.CH1, Raven.MotorMode.POSITION) # Set motor mode to POSITION
raven_board.set_motor_pid(Raven.MotorChannel.CH1, p_gain = 100, i_gain = 0, d_gain = 0, percent = 20) # Set PID values and 20% effort to reduce speed

# Make the motor spin until 4400 counts (10 rev of wheel motor)
raven_board.set_motor_target(Raven.MotorChannel.CH1, 4400)
```

##### Velocity controlled
In velocity controlled mode, you also get to set PID value and target in encoder counts per second.

```python
raven_board.set_motor_encoder(Raven.MotorChannel.CH1, 0) # Reset encoder
raven_board.set_motor_max_current(Raven.MotorChannel.CH1, 5) # Set motor current to 5 amps
raven_board.set_motor_mode(Raven.MotorChannel.CH1, Raven.MotorMode.VELOCITY) # Set motor mode to POSITION
raven_board.set_motor_pid(Raven.MotorChannel.CH1, p_gain = 10, i_gain = 0, d_gain = 0, percent = 20) # Set PID values and 20% effort to reduce acceleration

# Make the motor spin at -4400 counts/second (-10 rev/sec of wheel motor)
raven_board.set_motor_target(Raven.MotorChannel.CH1, -4400)
```

## Servos
Servos are position controlled motors that can move from -90 degree to 90 degree. Their signal is based on a [timed pulse](https://en.wikipedia.org/wiki/Servo_control). Typically, 1000us means -90 degree and 2000us means 90 degree. Some servo may have different values.

{{< figure src="/images/servo_timing.png" width="75%" >}}

Raven board supports up to [4 servos](../electrical#servo-connection). For each servo, you get to set the position in degree. Optionally, you can set the minimum and maximum pulse microsecond as `min_us` and `max_us` for you appropriate motor. Otherwise, they are defaulted to `min_us=1000` and `max_us=2000`. Here is an example of how to use them once you have [imported Raven](#import-raven).

```Python
# Set the servo 1 to -75 degrees with custom pulse microseconds
raven_board.set_servo_position(Raven.ServoChannel.CH1, -75, min_us=500, max_us=2500)
```

## Digital input/output
For switches/buttons or LED, Raven includes breakout to 5 Pi's [GPIO pins](https://github.com/MASLAB/kitbot-how-to?tab=readme-ov-file#digital-pins-connection). To use the pins, we have preinstalled `gpiozero` on the Pi. The documentation for `gpiozero` can be found here: https://gpiozero.readthedocs.io/en/stable/ 

Here are a couple useful examples:
1. Button (also limit switch) - https://gpiozero.readthedocs.io/en/stable/api_input.html#button
1. Light sensor - https://gpiozero.readthedocs.io/en/stable/api_input.html#lightsensor-ldr
1. LED - https://gpiozero.readthedocs.io/en/stable/api_output.html#led

We also have CircuitPython `digitalio` preinstalled. Here are a few other useful examples:
1. Break beam - https://learn.adafruit.com/ir-breakbeam-sensors/circuitpython
1. Ultrasonic distance sensor - https://github.com/adafruit/Adafruit_CircuitPython_HCSR04

## Other sensors
Raven board also includes [2 qwiic connectors](https://github.com/MASLAB/kitbot-how-to?tab=readme-ov-file#qwiic-connection) to use sensor boards with qwiic connections. For example, [Adafruit's AS7341 color sensor board](https://learn.adafruit.com/adafruit-as7341-10-channel-light-color-sensor-breakout/overview). You can use it with any i2c devices with qwiic connector [breakout cables](https://www.sparkfun.com/flexible-qwiic-cable-breadboard-jumper-4-pin.html). If the board does not come with a qwiic connector but is I2C compatible (has SDA and SCL connection), you can still use it with a qwiic to male header cable or manual wiring (ask staff).

> [!IMPORTANT]
> The default qwiic connector is the one nearest to the [motor encoder pins](../electrical#encoder-pins). It is labeled `I2C1`. Talk to staffs if you need to use the secondary connector near the [servo pins](../electrical#servo-pins).

The Pi comes with Adafruit's Circuit Python library preinstalled such that it is compatible with Adafruit's Python libraries for communicating with the sensor board. Here are a few useful sensors that we may stock:

### Adafruit AS7262 color sensor
This is a powerful color sensor that can sense red, orange, yellow, green, blue, and violet. It also has a built in LED to make color sensing more consistent. This can be useful to verify color of a game piece at the robot's intake or where the camera cannot see. 

#### Installation
```shell
sudo pip3 install adafruit-circuitpython-as726x --break-system-packages
```

#### Usage
```python
import board
from adafruit_as726x import AS726x_I2C

i2c = board.I2C()
sensor = AS726x_I2C(i2c)

while True:
    # Wait for data to be ready
    while not sensor.data_ready:
        time.sleep(0.1)

    # Print red value
    print("\n")
    print(f"Red: {sensor.red}")

    time.sleep(1)
```

#### Details
https://learn.adafruit.com/adafruit-as7262-6-channel-visible-light-sensor/overview

### Adafruit BNO085 IMU
[IMUs](https://en.wikipedia.org/wiki/Inertial_measurement_unit) are important to estimate how fast your robot is moving and rotating. Typical IMU measures the linear acceleration using an accelerometer and rotational velocity using a gyroscope. This is very useful for figuring where your robot is heading for accurate turning and straight driving manuever. The Adafruit BNO085 is special as it can gives you the orientation of the sensor without you having to manually integrate the rotational velocity. The BNO085 provides the orientation using [quaternion](https://en.wikipedia.org/wiki/Quaternions_and_spatial_rotation), an angle representation that is useful for graphics and robotics but less intuitive. This can be converted to Euler rotation (roll, pitch, and yaw).

#### Installation
```shell
sudo pip3 install adafruit-circuitpython-bno08x --break-system-packages
```

#### Usage
```python
import time
import board
import busio
import adafruit_bno08x
from adafruit_bno08x.i2c import BNO08X_I2C

i2c = busio.I2C(board.SCL, board.SDA, frequency=800000)
bno = BNO08X_I2C(i2c)

from adafruit_bno08x import BNO_REPORT_ROTATION_VECTOR

bno.enable_feature(BNO_REPORT_ROTATION_VECTOR) # Enable rotation vector

# Calculate yaw from quaternion
def find_heading(dqw, dqx, dqy, dqz):
    norm = sqrt(dqw * dqw + dqx * dqx + dqy * dqy + dqz * dqz)
    dqw = dqw / norm
    dqx = dqx / norm
    dqy = dqy / norm
    dqz = dqz / norm

    ysqr = dqy * dqy

    t3 = +2.0 * (dqw * dqz + dqx * dqy)
    t4 = +1.0 - 2.0 * (ysqr + dqz * dqz)
    yaw_raw = atan2(t3, t4)
    yaw = yaw_raw * 180.0 / pi
    if yaw > 0:
        yaw = 360 - yaw
    else:
        yaw = abs(yaw)
    return yaw  # heading in 360 clockwise


while True:
    # Print heading (yaw) with quaternion rotation vector
    quat_i, quat_j, quat_k, quat_real = bno.quaternion
    heading = find_heading(quat_real, quat_i, quat_j, quat_k)
    print("Heading:", heading) 

    print("")
    time.sleep(0.1)
```

#### Details
https://learn.adafruit.com/adafruit-9-dof-orientation-imu-fusion-breakout-bno085/overview

### VL53L0X / VL53L1X distance sensor
We may also stock some Time of Flight ([ToF](https://en.wikipedia.org/wiki/Time_of_flight)) sensors that send out a beams of light and measure the time it takes to bounce back to sense distance. This can be very useful to sense objects near the robot once the object goes out of the camera's field of view. We may have these sensors from Adafruit with qwiic connector, or from Pololu with header pins. Adafruit's library can be used for both of these. 

#### Installation
VL53L0X
```shell
sudo pip3 install adafruit-circuitpython-vl53l0x --break-system-packages
```

VL53L1X
```shell
sudo pip3 install adafruit-circuitpython-vl53l1x --break-system-packages
```

#### Usage
VL53L0X
```python
import time
import board
from adafruit_vl53l0x import VL53L0X

i2c = board.I2C()
vl53 = VL53L0X(i2c)

while True:
    # Print distance
    print(f"Range: {vl53.range}mm")
    time.sleep(1.0)
```

VL53L1X
```python
import time
import board
from adafruit_vl53l1x import VL53L1X

i2c = board.I2C()
vl53 = VL53L1X(i2c)
vl53.start_ranging()

while True:
    # Print distance
    if vl53.data_ready:
        print(f"Distance: {vl53.distance} cm")
        vl53.clear_interrupt()
        time.sleep(1.0)
```

#### Details
https://learn.adafruit.com/adafruit-vl53l0x-micro-lidar-distance-sensor-breakout/overview
https://learn.adafruit.com/adafruit-vl53l1x/overview 

## Telemetry
It's often nice to be able to quickly read telemetry from the robot, either for debugging or monitoring purposes. The MASLAB library ships with a small module called `streamer` that exposes a web streaming service, allowing you to visualize simple telemetry data, computer vision outputs, and robot odometry without an RDP connection.

Initialize Streamer as follows:
```python
from streamer import Streamer
s = Streamer()
```
This should start a web server. It should print out something like this:
```
!! MASLAB streamer is running !!
 * Serving Flask app 'streamer.server'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5000
 * Running on http://10.31.141.10:5000
Press CTRL+C to quit
```
Navigate to the address that it displays (in the above case, http://10.31.141.10:5000) to see the web interface. Right now, it should be blank. The following functions are used to display data:
- `s.set_data(data: Dict)`: write any kind of text data as key-value pairs.
- `s.set_img(img: cv2.Mat)`: display an OpenCV image.
- `s.update_odom_state(x: float, y: float, theta: float)`: set the current estimated robot position and heading (in radians).
- `s.update_circles(circles: List[Tuple(float, float, str)])`: Render circles, for example the can positions. Each entry must be of the form `(x, y, c)` where `x` and `y` are the position and `c` is the color. Any [color that HTML recognizes](https://en.wikipedia.org/wiki/Web_colors) can be used.
- `s.update_lines(lines: List[Tuple(float, float, float, float, str])`: Render lines. Each entry must be of the form `(x1, y1, x2, y2, c)`.

Here is a minimal example:
```python
from server import Streamer
import time
import cv2
import random
import math

stream = Streamer()

pos = [0, 0, random.uniform(0, 2*math.pi)]  # x, y, theta

# display an image
stream.set_img(cv2.imread("test.jpg"))

# continuously update data and odometry
curr_data = {}
while True:
    pos[0] += math.cos(pos[2]) * 0.01
    pos[1] += math.sin(pos[2]) * 0.01
    pos[2] += random.uniform(-0.2, 0.2)
    curr_data["timestamp"] = time.time()
    curr_data["random_value"] = random.random()

    # stream data and odometry
    stream.set_data(curr_data)
    stream.update_odom_state(pos[0], pos[1], pos[2])
    stream.update_circles(
        [
            (0.40, 0.20, "red"),
            (0.13, -0.40, "green"),
        ]
    )
    stream.update_lines(
        [
            (0, 0, 0.75, 0.75, "blue"),
        ]
    )
    time.sleep(0.05)
```

The interface should look something like this:

## What's next?
Now that you have the software and firmware to run Raven. **AFTER** the battery lecture and reviewing the [battery guide](../battery). Feel free to power your Pi [with the battery](../battery#using-pi-with-battery) and see if you can make your motors and servos move!

Once you are familiar with using the Pi and the MASLAB library to interact with Raven, you can start thinking about writing and testing out your robot software. To keep your software development progress organized, we will be using [Git version control system](https://en.wikipedia.org/wiki/Git). Checkout [Git guide](../git) to "git" started.
