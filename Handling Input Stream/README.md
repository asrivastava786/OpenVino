# Handling Input Streams

> Implement a function that can handle camera image, video file or webcam inputs


![](Input.gif)

The main thing here is just to check the `input` argument passed to the command line.

1) The argument parser makes note that "CAM" is an acceptable input meaning to use the webcam. In that case, the `input_stream`
should be set to `0`, as `cv2.VideoCapture()` can use the system camera when set to zero.

2) Checking whether the input name is a filepath containing an image file type, such as `.jpg` or `.png`. If so, you'll just set the `input_stream` to that path. You should also set the flag here to note it is a single image, so you can save down the image as part of one of the later steps.

3) It's mostly the same as the image, as the `input_stream` is the filepath passed to the `input` argument, but you don't need to use a flag here.

4) Exception handling - does your app just crash if the input is invalid or missing, or does it still log useful information to the user?

> Use `cv2.VideoCapture()` and open the capture stream

```
capture = cv2.VideoCapture(input_stream)
capture.open(args.input)

while capture.isOpened():
    flag, frame = cap.read()
    if not flag:
        break
```

It's a bit outside of the instructions, but it's also important to check whether a key gets 
pressed within the while loop, to make it easier to exit. 

You can use:
```
key_pressed = cv2.waitKey(60)
```
to check for a key press, and then
```
if key_pressed == 27:
    break
```
to break the loop, if needed. Key 27 is the Escape button.

> Re-size the frame to 100x100

```
image = cv2.resize(frame, (100, 100))
```

> Add Canny Edge Detection to the frame with min & max values of 100 and 200, respectively

Canny Edge detection is useful for detecting edges in an image, and has been a useful
computer vision technique for extracting features. This was a step just so you could get a little
more practice with OpenCV.

```
edges = cv2.Canny(image,100,200)
```

> Display the resulting frame if it's video, or save it if it is an image

For video:
```
cv2.imshow('display', edges)
```
For a single image:
```
cv2.imwrite('output.jpg', edges)
```

> Close the stream and any windows at the end of the application

Make sure to close your windows here so you don't get stuck with them on-screen.

```
capture.release()
cv2.destroyAllWindows()
```
