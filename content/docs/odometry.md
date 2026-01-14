---
title: "Odometry"
weight: 8
keywords:
- location
- encoder
- frame
- position
- heading
# bookComments: false
# bookSearchExclude: false
# bookPostThumbnail: thumbnail.*
---

# Odometry
Now that your robot can move itself, it's also time to let it track where it is in the field! 

## Wheel odometry
The motor provided in your kit should have [encoder](../software#encoder) to estimate how much the motor has turned. From those, we can get a decent estimate of where the robot is with some trigonometry. This is assuming that:
* The robot moves slow enough such that the wheels do not skid and slip on the floor 
* Our code updates fast enough such that the encoder counts' changes are small

We will also need to know:
* Robot wheel diameter
* Robot track width (distance between center of both wheel)
* Motor CPR (count per rotation = motor gear ratio * encoder count per revolution)

Here is a base class to get you started. Your task is to update the `TODO` to estimate changes in x, y, and theta. 

> [!NOTE]
> Keep in mind that your estimate changes (particularly in x and y) should be on the world frame:
> {{< figure src="/images/robot_frame.svg" width="75%" >}}

> [!TIP]
> Here is a pretty good walk through for wheel odometry: https://medium.com/@nahmed3536/wheel-odometry-model-for-differential-drive-robotics-91b85a012299

```python
import numpy as np


class WheelOdometry:
    def __init__(
        self,
        wheel_diameter: float,
        track_width: float,
        cpr: int,
        left_encoder_init: int = 0,
        right_encoder_init: int = 0,
    ):
        self.reset()
        # Robot properties
        self.__WHEEL_DIAMETER = wheel_diameter
        self.__TRACK_WIDTH = track_width
        self.__WHEEL_MOTOR_MPC = self.__WHEEL_DIAMETER * np.pi / cpr  # Distance per count
        self.__left_encoder = left_encoder_init
        self.__right_encoder = right_encoder_init

    def reset(self):
        self.__x = 0.0
        self.__y = 0.0
        self.__theta = 0.0

    @property
    def x(self) -> float:
        return self.__x

    @property
    def y(self) -> float:
        return self.__y

    @property
    def theta(self) -> float:
        return self.__theta

    def __repr__(self):
        return f"x: {self.x}\ny: {self.y}\nheading: {self.theta * 180/np.pi} degree"

    def update(self, left_encoder, right_encoder):
        # Get encoder change
        d_left_encoder = left_encoder - self.__left_encoder
        d_right_encoder = right_encoder - self.__right_encoder

        # Update encoder values
        self.__left_encoder = left_encoder
        self.__right_encoder = right_encoder

        # Get distance change
        d_left_distance = d_left_encoder * self.__WHEEL_MOTOR_MPC
        d_right_distance = d_right_encoder * self.__WHEEL_MOTOR_MPC

        ###### TODO: Calculate changes ###### 
        d_theta = 0 
        d_x = 0
        d_y = 0
        ###### End of TODO ######

        # Update reading
        self.__theta = (self.__theta + d_theta + np.pi) % (
            2 * np.pi
        ) - np.pi  # Wrapping to 2*pi
        self.__x += d_x
        self.__y += d_y
```

Once you fully implemented the WheelOdometry class, you can use it like this:

```python
# Update with correct robot values
wheel_odom = WheelOdometry(
                wheel_diameter=1,
                track_width=1,
                cpr=1000,
                left_encoder_init=0,
                right_encoder_init=0,
            ) 

# Update with encoder count
wheel_odom.update(
    left_encoder=123,
    right_encoder=456,
)

print(wheel_odom.x) # Print x coordinate of robot
print(wheel_odom.y) # Print y coordinate of robot
print(wheel_odom.theta) # Print angle of robot (radian)

print(wheel_odom) # Print all
```
