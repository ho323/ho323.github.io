---
title: "OpenCV를 이용한 영상처리"
categories: Python
tags:
  - Python
  - OpenCV
search: true
---

나는 자율주행 인공지능을 공부하고 싶다.  
자율주행 인공지능은 카메라를 통해 촬영하는 영상을 인식해야하기 때문에 영상을 처리하는 방법을 알고싶었다.   

그러던 중 아래 링크를 발견했다.  
[Read, Write and Display a video using OpenCV |](https://learnopencv.com/read-write-and-display-a-video-using-opencv-cpp-python/)

그래서 위 글을 간략하게 리뷰해보면서 OpenCV를 통한 영상처리에 대해 공부해보려고 한다.  

## 비디오란?
![The-Horse-in-Motion-anim](https://user-images.githubusercontent.com/86637300/137582120-78d33600-f1ca-4d0e-8215-f4a8d202b432.gif)
먼저, 비디오란 무엇인가?  
비디오는 이미지 시퀀스 즉, 사진들을 연속적으로 나열해서 빠르게 전환시켜 보이게되는 것이다.  

여기서 이미지가 얼마나 빨리 전환되는지 측정하는 것은 초당 프레임(FPS)라는 것이다.  
FPS가 40이라는 말은 초당 40개의 이미지가 표시된다는 의미이다. 


## 비디오 읽어오기
비디오 파일을 읽기 위한 첫 번째 단계는 VideoCapture 객체를 만드는 것이다.  
이 인수는 장치(카메라)의 index가 되거나 비디오 파일의 이름이 될 수 있다.   

영상 출처: [Puppies Dogs Friendship - Free video on Pixabay](https://pixabay.com/videos/puppies-dogs-friendship-joy-69168/)   
```python
import cv2
import os

default_dir = os.getenv('HOME')
video_path = os.path.join(default_dir, 'Puppies.mp4')

# Create a VideoCapture object and read from input file
# If the input is the camera, pass 0 instead of the video file name
cap =  cv2.VideoCapture(video_path)
```


## 비디오 표시하기
VideoCapture 객체가 생성된 후 프레임별로 영상을 표시할 수 있다.  
그리고 비디오의 프레임은 단순히 이미지이므로 imshow() 함수를 사용하여 표시할 수 있다.  

비디오에서 imshow() 함수 뒤에 waitKey() 함수로 각 프레임을 일시정지한다.  
이미지의 경우, waitKey() 함수에 0을 넣지만, 비디오 재생을 위해서는 0보다 큰 숫자를 넣어야 한다.  
0은 무한한 시간 동안 일시정지하기 때문이다.  
이 숫자는 각 프레임이 표시되기를 원하는 시간(ms)과 같다.  

웹켐에서는 1을 사용하는 것이 적절하다고 한다.  
왜냐하면 waitKey에서 지연을 1ms로 지정하더라도 디스플레이 프레임 속도가 웹캠의 프레임 속도에 의해 제한되기 때문이다.  
```python
# Check if camera opened successfully
if (cap.isOpened()== False):
    print(“Error opening video stream or file”)

# Read until video is completed
while(cap.isOpened()):
    # Capture frame-by-frame
    ret, frame = cap.read()
    if ret == True:
    # Display the resulting frame
        cv2.imshow(‘Frame’,frame)
        # Press Q on keyboard to  exit
        if cv2.waitKey(1) & 0xFF == ord(‘q’):
            break
        # Break the loop
        else:
            break

# When everything done, release the video capture object
cap.release()

# Closes all the frames
cv2.destroyAllWindows()
```
<img width="956" alt="Frame" src="https://user-images.githubusercontent.com/86637300/137582113-4c0c6a31-e7f8-4a05-8d7a-badbdb132c5e.png">
(재생됨)

## 비디오 작성하기
그 다음 단계는 비디오를 저장하는 것이다.  

이미지의 경우 cv2.imwrite()를 사용하기만 하면 된다.  
하지만 동영상의 경우 VideoWriter 객체를 만들어야 한다.  

먼저, 출력 파일 이름을 지정해야 한다.  
그 다음, FourCC 코드와 FPS를 지정해야 한다.  
마지막으로 프레임 크기를 지정해야 한다.  

FourCC는 비디오 코덱을 지정하는 데 사용되는 4byte 코드다.  
사용 가능한 코드 목록은 fourcc.org 에서 찾을 수 있다.  

하지만 FourCC의 일부 코드는 시스템에 따라 작동이 되지 않을 수도 있다.  
MJPG는 안전한 선택이라고 한다.  
```python
# Define the codec and create VideoWriter object.The output is stored in 'outpy.avi' file.
# Define the fps to be equal to 10. Also frame size is passed.

out = cv2.VideoWriter('outpy.avi',cv2.VideoWriter_fourcc('M','J','P','G'), 10, (frame_width,frame_height))
```
<img width="597" alt="Output" src="https://user-images.githubusercontent.com/86637300/137582105-fa6c0a5e-e88b-4703-bed4-f9c0aec89ace.png">


## 마무리
최종적으로 코드를 작성하면 아래와 같다.  
```python
import cv2
import os
import numpy as np
 
default_dir = os.getenv('HOME')
video_path = os.path.join(default_dir, 'Puppies.mp4')

# Create a VideoCapture object and read from input file
# If the input is the camera, pass 0 instead of the video file name
cap =  cv2.VideoCapture(video_path)

# Check if camera opened successfully
if (cap.isOpened() == False):
    print("Unable to read camera feed")
 
# Default resolutions of the frame are obtained.The default resolutions are system dependent.
# We convert the resolutions from float to integer.
frame_width = int(cap.get(3))
frame_height = int(cap.get(4))

# Define the codec and create VideoWriter object.The output is stored in ‘outpy.avi’ file.
out = cv2.VideoWriter(‘outpy.avi’,cv2.VideoWriter_fourcc(‘M’,’J’,’P’,’G’), 120, (frame_width,frame_height))
while(True):
    ret, frame = cap.read()
    if ret == True:
        # Write the frame into the file ‘output.avi’
        out.write(frame)
        # Display the resulting frame   
        cv2.imshow(‘frame’,frame)
        # Press Q on keyboard to stop recording
        if cv2.waitKey(1) & 0xFF == ord(‘q’):
            break
        # Break the loop
        else:
            break 

# When everything done, release the video capture and video write objects
cap.release()
out.release()

# Closes all the frames
cv2.destroyAllWindows()
```

이렇게 비디오를 읽고, 쓰고, 표시하는 방법을 통해서 자율주행과 같은 컴퓨터 비전과 머신러닝 학습에 응용할 수 있을 것이다.  
