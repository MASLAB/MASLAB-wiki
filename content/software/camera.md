---
title: "Camera"
weight: 7
keywords:
- opencv
- homography
- hsv
- brightness
- exposure
- white balance
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

# Camera
Now that your robot can move itself, it's time to give it some vision to understand its surrounding!

## Prerequisites
1. Computer with [Python installed](https://www.python.org/downloads/). Python is already installed by default on the Pi.
1. Webcam (provided USB webcam or laptop's built-in)

> [!TIP]
> The following tutorial can generally be followed on your laptop using either the provided USB webcam or built-in webcam. For consistency between what you see while running on your laptop and while running on the Pi, we would recommend using the provided webcam. Using the built-in webcam is fine for testing out small things only.

## OpenCV
[OpenCV](https://opencv.org/) (Open Computer Vision) has been the de-facto software library for doing image processing (at least at MASLAB). 

### OpenCV for Python installation
#### On the Pi
```shell
sudo apt install -y python3-opencv
```

#### On your computer
```shell
pip install opencv-python
```

### OpenCV import
To add OpenCV to your Python script, import it with:
```python
import cv2 # OpenCV version 2
import numpy as np # Numpy numerical library for mathematical operation on image data
```

## Basic camera usage
With OpenCV [installed and imported](#opencv), you can let OpenCV capture what your camera see with `cv2.VideoCapture()`. In general, if the computer only has 1 camera installed, you can create a video capture device with:
```python
cap = cv2.VideoCapture(0) # Capture from first camera
```

In this code, the `0` indicates the camera index. For a simple single camera system, this is just the first camera that the system recognizes. If you are running this on your laptop, this means the built-in webcam (if any). If you attach the USB camera to the Pi and run on the Pi, it _should_ be the USB camera.

If you are using the USB camera and want to try on your laptop and not the Pi, you may have to add additional arguments.

For Windows:
```python
cap = cv2.VideoCapture(1, cv2.CAP_DSHOW)
```

For Linux:
```python
cap = cv2.VideoCapture(1, cv2.CAP_V4L2)
```

For Mac:
```python
cap = cv2.VideoCapture(1, cv2.CAP_AVFoundation)
```

With these, the `cv2.CAP_xxx` indicates the API needed to access the USB camera. This is different with different operating system. `1` indicates the next camera besides the built-in camera.

To get the exact API (backend) and the index, `cv2-enumerate-cameras` is a great tool: https://github.com/lukehugh/cv2_enumerate_cameras

> [!TIP]
> If you are running OpenCV on your laptop, you may need to allow Python to access the camera in your security settings and/or with your Antivirus software (if any).

### Read camera frame
To read a frame from the video capture device, we can use `cv2.read()`:
```python
ret, frame = cap.read()
```

`cap.read()` returns 2 value, `ret` and `frame`. `ret` is a debugging value that will be `True` if a frame was captured successfully and `False` otherwise. If `ret == True`, `frame` is a valid OpenCV frame that can be processed.

### Write camera frame
We can then save the frame with `cv2.imwrite`:
```python
cv2.imwrite("test_frame.png", frame) # Save frame to path "test_frame.png"
```

With this code, `cv2.imwrite` will save the captured `frame` to an image called `test_frame.png`. You can redirect it to another folder to keep things organized by creating a folder and change the path. i.e. `debug/test_frame.png`. Note that you will have to create the folder manually first. `imwrite` will **silently** not write to non-existent folder.

### View camera in real-time
For more real-time debugging, you can run a loop with `cv2.imshow()` to continuously display the frame:
```python
# Adapted from https://docs.opencv.org/4.x/dd/d43/tutorial_py_video_display.html
while True:
    # Capture frame-by-frame
    ret, frame = cap.read()
 
    # if frame is read correctly ret is True
    if not ret:
        print("Can't receive frame (stream end?). Exiting ...")
        break

    cv2.imshow('frame', frame)
    if cv2.waitKey(1) == ord('q'):
        break
```

With this code, an OpenCV window will pop up, showing the camera feed in real-time. `cv2.waitKey(1) == ord('q')` tells OpenCV to listen for `q` key press for `1` millisecond. So to quit the loop, press `q` on your keyboard. Closing the window will only make OpenCV pop another window up.

> [!NOTE]
> For running `cv2.imshow` on the Pi, a graphical window is needed such that [SSH](../raspberry-pi#connecting-over-ssh) remote connection alone will not be able to display from the Pi. Therefore, you will need to test with [RDP](../raspberry-pi#connecting-over-rdp). 

## Basic image processing
Now that you are able to get frames from the camera, you can do some basic image processing. 

### Color space
With image processing, it is important to understand how color is represented. By default, an OpenCV frame is encoded as an (height x width x 3) array. This array contains (height x width) pixels. Each pixel is encoded as an array of 3 values from 0 to 255 of \[blue, red, green\] channels (as opposed to the usual RGB). This is one [color space](https://www.geeksforgeeks.org/python/color-spaces-in-opencv-python/) that OpenCV understands and easy for computer to understand. However, for object recognition based on color shades, it is difficult to compare. 

For example, `bgr(191, 112, 43)` to `bgr(89, 52, 20)` may come from the same object at different lighting angles yet have wildly different RGB values.

{{< figure src="/images/blue_shades.png" width="50%" >}}

A better color space to compare would be [HSV](https://en.wikipedia.org/wiki/HSL_and_HSV). HSV is based on human perception of color and encodes color in 3 values: 
* H - Hue: color shade as represented in a circle (0-179) 
* S - Saturation: color vibrancy (0-255)
* V - Value: color brightness (0-255)

With HSV, the two previous colors become `hsv(150, 198, 191)` and `hsv(150, 198, 89)`. In this case, the colors only differ by the brightness value as expected.

To convert from one color-space to another, `cv2.cvtColor()` can be used:
```python
hsv_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
```

With this, `cv2.cvtColor` will convert `frame` to `hsv_frame` from the default BGR to HSV color space as specified by `cv2.COLOR_BGR2HSV`.

> [!NOTE]
> `cv2.imwrite` and `cv2.imshow` expects BGR by default. Therefore, using it on an HSV frame may result in some very \~artistic\~ images.

### Basic examples
Here are a few quick examples to get you started:

1. [Getting Started with Python OpenCV](https://www.geeksforgeeks.org/python/getting-started-with-python-opencv/). Fundamental image operations. Feel free to save a frame from the camera to follow along.
1. [Color segmentation](https://github.com/MASLAB/cv-samples/blob/master/color_segmentation.py). From MASLAB 2020 to detect the biggest continuous green object. You may need to adjust the HSV mask correspondingly.
1. [Object distance detection](https://github.com/MASLAB/opencv-example/tree/master?tab=readme-ov-file). More advanced object distance detection from 2025, following the [lecture slide](https://maslab.mit.edu/2025/files/lecture8-slides.pdf).

### Dilation and Convex Hull
This year, the objects you'll be manipulating don't have nice solid colors. If you just run color thresholding, you might end up with a bunch of disconnected regions. To get around this, we can run dilation, which basically expands each region to "merge together" regions that are close together. However, this changes the exterior boundaries of the detected region. We therefore might want to run erosion, which shrinks the region boundary. We can do both of these with OpenCV:

```python
kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (10, 10))
thresh = cv2.dilate(thresh, kernel, iterations=2)
thresh = cv2.erode(thresh, kernel, iterations=1)
```

We should get a result that looks like this:

{{< figure width="50%" alt="image" src="/images/can_threshold.png" >}}

Original thresholding (left), after dilation/erosion (right)

This might be good enough for your algorithm! But even with dilation and erosion, we still see some interesting concavities inside the cans. We can get rid of these with contour detection and convex hulling:

```python
contours, _ = cv2.findContours(thresh, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
contours = [cv2.convexHull(contour) for contour in contours]
```

Now, the geometry is better handled. This might help, for example, in heuristics that determine the nearest can.

## Advanced camera usage
### Manual settings
Laptop webcams and USB webcams are designed for very general usage such that there may be automatic settings such as exposure and white balance. These may be nice to have for changing environments but may trigger randomly and mess up your image and color calibration. Therefore, it may help to manually control these.

#### Manual exposure
```python
cap.set(cv2.CAP_PROP_AUTO_EXPOSURE, 0.25)  # Set to manual exposure mode with 0.25 "magic number"
cap.set(cv2.CAP_PROP_EXPOSURE, -7)  # Set exposure time to 2^-7 = 1/128 second
```

#### Manual white balance
```python
cap.set(cv2.CAP_PROP_AUTO_WB, 0.0) # Disable auto white balance
cap.set(cv2.CAP_PROP_WB_TEMPERATURE, 4200) # Set white balance temperature to 4200K
```

#### Higher resolution
By default, OpenCV will capture video at 640x480 resolution. This should be okay and fast for most applications. However, if you want to trade speed for accuracy, you can up the resolution with:
```python
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 1920)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 1080)
cap.set(cv2.CAP_PROP_FOURCC, cv2.VideoWriter_fourcc(*"MJPG"))
```
This requests the camera to run at the maximum 1920x1080 resolution with compressed motion JPG data for faster read.

#### Other props
Along with exposure and white balance, there are other properties that you may want to explore. They can be found here: https://docs.opencv.org/4.x/d4/d15/group__videoio__flags__base.html#ggaeb8dd9c89c10a5c63c139bf7c4f5704da5c790a640b290f73fcd279e8bbaf0f13

> [!TIP]
> Unfortunately the official documentation is quite poor in describing the values so you may have to rely a lot on other online resources / forums / [OpenCV GitHub issues](https://github.com/opencv/opencv/issues)

### Calibration
Due to the camera lens, captured images may be distorted, making straight features curved and curved features straight. For example:

{{< figure src="/images/distorted_image.png" >}}

This makes it difficult to accurately estimate the true object position as image processing algorithms often assume the inputs are non-distorted images. 

To correct the image, a calibration procedure is necessary to find how distorted the camera is and calculate a set of correction values to be applied after. This procedure can be found along with more detailed explanation for camera distortion at https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html

#### Calibration pattern
You will need a calibration pattern of checkered squares. Here is an example one with 2 x 2 cm squares: [calibration.pdf](file/calibration.pdf). When printing, make sure to apply no scaling to get the correct square size. Once you have a pattern, make sure to tape it down on a surface as flat as possible.

### Homography
We can use some transformation to figure out how far away is an object in the picture just from their pixel location. This technique is called [homography](https://en.wikipedia.org/wiki/Homography_%28computer_vision%29). With a well calibrated camera, we can figure out the transformation and get a good estimate of the location of cans and goals on the image.

The following examples were adapted from a section of MIT Robotics: Science and Systems course on visual servoing: https://github.com/mit-rss/visual_servoing 

#### Data collection
To get the transformation, we need to collect some points (at least 3, as many as you want for higher accuracy). We can use either the Pi or the computer to get the data.

First, make sure that the camera is fixed to the robot and will not tilt around later because the data we will collect is specific to the camera location and orientation. Don't just tape it! ~~like your lazy MASLAB TA~~. Also make sure the camera is mounted somewhere high and tilted down to look at the floor.

Then place some grid pattern down to help with getting the location in the 3D world. For this tutorial, we will follow [ROS convention](https://www.ros.org/reps/rep-0103.html) for robot coordinate system (x forward, y left, z up).

{{< figure src="/images/homography_calibration.jpg" width="75%" >}}

Once you are ready to take measurements, use this script with the camera attached on the robot to collect points on image frame. Check [basic camera usage](#basic-camera-usage) for how to select correct camera.

```python
import cv2  # OpenCV version 2

IMAGE_WINDOW_NAME = "calibration"

# TODO: Change to your appropriate camera
cap = cv2.VideoCapture(0)  

# Take a picture and show frame
ret = False
while not ret:
    ret, frame = cap.read()
    cap.release()
cv2.imshow(IMAGE_WINDOW_NAME, frame)


# Mouse click event listener
def mouse_event_listener(event, u, v, flags, param):
    frame = param
    # For left mouse click
    if event == cv2.EVENT_LBUTTONDOWN:
        # Print clicked coordinate
        print(f"{mouse_event_listener.count} - u: {u}, v: {v}")
        cv2.circle(frame, (u, v), 10, (0, 0, 255), 2)  # Draw red circle
        # Print point order
        cv2.putText(
            frame,
            str(mouse_event_listener.count),
            (u + 10, v - 10),
            cv2.FONT_HERSHEY_PLAIN,
            2,
            (0, 0, 255),
            2,
        )
        cv2.imshow(IMAGE_WINDOW_NAME, frame)
        # Increment click count
        mouse_event_listener.count += 1


# Initialize click count
mouse_event_listener.count = 0

# Set click callback
cv2.setMouseCallback(IMAGE_WINDOW_NAME, mouse_event_listener, frame)

# Wait for q or close to quit
while True:
    if cv2.waitKey(1) & 0xFF == ord("q"):
        break
    try:
        cv2.getWindowProperty(IMAGE_WINDOW_NAME, 0)
    except cv2.error:
        break
cv2.destroyAllWindows()
```

This script will take a picture from the camera and show it. When you click on any part of the picture, your terminal will print the pixel coordinate of the clicked point. It will also update the image with clicked points. Use this to get a record of pixel coordinates and the corresponding real-world coordinates. For example, this is what get printed when these points are clicked:

{{< figure src="/images/homography_clicks.png" width="75%" >}}

```shell
0 - u: 113, v: 305
1 - u: 241, v: 274
2 - u: 487, v: 183
3 - u: 111, v: 166
4 - u: 411, v: 247
5 - u: 554, v: 303
6 - u: 296, v: 167
```

For reference, the image coordinate system is as followed:
{{< figure src="/images/image_coordinate.svg" width="50%" >}}

#### Homography calculation
Now that we have the data, let's use it to calculate a homography matrix and estimate real-world coordinates from image.

```python
import cv2
import numpy as np

IMAGE_WINDOW_NAME = "estimation"

# TODO: Change to your appropriate image points
PTS_IMAGE_PLANE = [
    [113, 305],
    [241, 274],
    [487, 183],
    [111, 166],
    [411, 247],
    [554, 303],
    [296, 167],
]

# TODO: Change to your appropriate real-world points
PTS_GROUND_PLANE = [
    [1, 3],
    [2, 1],
    [6, -4],
    [7, 4],
    [3, -2],
    [1, -4],
    [7, 0],
]

# TODO: Change to your appropriate camera
cap = cv2.VideoCapture(0)

# Take a picture and show frame
ret = False
while not ret:
    ret, frame = cap.read()
    cap.release()
cv2.imshow(IMAGE_WINDOW_NAME, frame)


def transform_uv_to_xy(h, u, v):
    """
    u and v are pixel coordinates.
    The top left pixel is the origin, u axis increases to right, and v axis
    increases down.

    Returns a normal non-np 1x2 matrix of xy displacement vector from the
    camera to the point on the ground plane.
    Camera points along positive x axis and y axis increases to the left of
    the camera.

    Units are in whichever unit h was calculated in.
    """
    homogeneous_point = np.array([[u], [v], [1]])
    xy = np.dot(h, homogeneous_point)
    scaling_factor = 1.0 / xy[2, 0]
    homogeneous_xy = xy * scaling_factor
    x = homogeneous_xy[0, 0]
    y = homogeneous_xy[1, 0]
    return x, y


np_pts_ground = np.array(PTS_GROUND_PLANE)
np_pts_ground = np.float32(np_pts_ground[:, np.newaxis, :])

np_pts_image = np.array(PTS_IMAGE_PLANE)
np_pts_image = np.float32(np_pts_image[:, np.newaxis, :])

h, err = cv2.findHomography(np_pts_image, np_pts_ground)


# Mouse click event listener
def mouse_event_listener(event, u, v, flags, param):
    frame = param
    # For left mouse click
    if event == cv2.EVENT_LBUTTONDOWN:
        # Transform to x y (world coordinate)
        x, y = transform_uv_to_xy(h, u, v)
        print(f"x: {x}, y: {y:}")  # Print world coordinate
        test_frame = cv2.circle(
            frame.copy(), (u, v), 10, (0, 0, 255), 2
        )  # Draw red circle

        # Print world coordinate on picture
        cv2.putText(
            test_frame,
            f"x: {x:.2f} y: {y:.2f}",
            (u - 120, v - 20),
            cv2.FONT_HERSHEY_PLAIN,
            2,
            (0, 0, 255),
            2,
        )
        cv2.imshow(IMAGE_WINDOW_NAME, test_frame)


# Set click callback
cv2.setMouseCallback(IMAGE_WINDOW_NAME, mouse_event_listener, frame)

# Wait for q or close to quit
while True:
    if cv2.waitKey(1) & 0xFF == ord("q"):
        break
    try:
        cv2.getWindowProperty(IMAGE_WINDOW_NAME, 0)
    except cv2.error:
        break
cv2.destroyAllWindows()

```

This example will calculate a homography transformation matrix from your collected points and provide a function to transform from image coordinate to world coordinate. When you click on any part of the picture, it will show the estimate real-world coordinate at the clicked point. For example, this is what is shown when a point is clicked:

{{< figure src="/images/homography_estimate.png" width="75%" >}}

```shell
x: 9.063606157653815, y: -3.9984504855440397
```

For reference, this example simply used the calibration squares as a unit of measurement. Your real-world points should be based on whichever unit system you plan to use and hopefully it is metric. 