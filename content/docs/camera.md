---
title: "Camera"
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

# Camera how-to
Now that your robot can move itself, it's time to give it some vision to understand its surrounding!

## Prerequisites
1. Computer with [Python installed](https://www.python.org/downloads/). Python is already installed by default on the Pi.
1. Webcam (provided USB webcam or laptop's built-in)

> [!TIP]
> The following tutorial can generally be followed on your laptop using either the provided USB webcam or built-in webcam. For consistency between what you see while running on your laptop and while running on the Pi, we would recommend using the provided webcam. Using the built-in webcam is fine for testing out small things only.

## OpenCV
[OpenCV](https://opencv.org/) (Open Computer Vision) has been the de-facto software library for doing /images processing (at least at MASLAB). 

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
import numpy as np # Numpy numerical library for mathematical operation on /images data
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

With this code, `cv2.imwrite` will save the captured `frame` to an /images called `test_frame.png`. You can redirect it to another folder to keep things organized by creating a folder and change the path. i.e. `debug/test_frame.png`. Note that you will have to create the folder manually first. `imwrite` will **silently** not write to non-existent folder.

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
> For running `cv2.imshow` on the Pi, a graphical window is needed such that [SSH](3-Raspberry-Pi#connecting-over-ssh) remote connection alone will not be able to display from the Pi. Therefore, you will need to test with [RDP](3-Raspberry-Pi#connecting-over-rdp). 

## Basic /images processing
Now that you are able to get frames from the camera, you can do some basic /images processing. 

### Color space
With /images processing, it is important to understand how color is represented. By default, an OpenCV frame is encoded as an (height x width x 3) array. This array contains (height x width) pixels. Each pixel is encoded as an array of 3 values from 0 to 255 of \[blue, red, green\] channels (as opposed to the usual RGB). This is one [color space](https://www.geeksforgeeks.org/python/color-spaces-in-opencv-python/) that OpenCV understands and easy for computer to understand. However, for object recognition based on color shades, it is difficult to compare. 

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

1. [Getting Started with Python OpenCV](https://www.geeksforgeeks.org/python/getting-started-with-python-opencv/). Fundamental /images operations. Feel free to save a frame from the camera to follow along.
1. [Color segmentation](https://github.com/MASLAB/cv-samples/blob/master/color_segmentation.py). From MASLAB 2020 to detect the biggest continuous green object. You may need to adjust the HSV mask correspondingly.
1. [Object distance detection](https://github.com/MASLAB/opencv-example/tree/master?tab=readme-ov-file). More advanced object distance detection from 2025, following the [lecture slide](https://maslab.mit.edu/2025/files/lecture8-slides.pdf).

## Advanced camera usage
### Manual settings
Laptop webcams and USB webcams are designed for very general usage such that there may be automatic settings such as exposure and white balance. These may be nice to have for changing environments but may trigger randomly and mess up your /images and color calibration. Therefore, it may help to manually control these.

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

#### Other props
Along with exposure and white balance, there are other properties that you may want to explore. They can be found here: https://docs.opencv.org/4.x/d4/d15/group__videoio__flags__base.html#ggaeb8dd9c89c10a5c63c139bf7c4f5704da5c790a640b290f73fcd279e8bbaf0f13

> [!TIP]
> Unfortunately the official documentation is quite poor in describing the values so you may have to rely a lot on other online resources / forums / [OpenCV GitHub issues](https://github.com/opencv/opencv/issues)

### Calibration
Due to the camera lens, captured images may be distorted, making straight features curved and curved features straight. For example:

{{< figure src="/images/distorted_image.png" >}}

This makes it difficult to accurately estimate the true object position as /images processing algorithms often assume the inputs are non-distorted images. 

To correct the /images, a calibration procedure is necessary to find how distorted the camera is and calculate a set of correction values to be applied after. This procedure can be found along with more detailed explanation for camera distortion at https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html

#### Calibration pattern
You will need a calibration pattern of checkered squares. Here is an example one with 2 x 2 cm squares: [calibration.pdf](file/calibration.pdf). When printing, make sure to apply no scaling to get the correct square size. Once you have a pattern, make sure to tape it down on a surface as flat as possible.
