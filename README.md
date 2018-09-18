# Basic OpenCV Tutorial

## 1.) ROS and OpenCV
You should already have OpenCV istalled from when you installed ROS. If you have not yet installed ROS please follow these instructions

 - Do _Desktop Install_
   - Follow until _Step 1.7_ at the end of the page

   First, install **ROS Melodic** using the following instructions: http://wiki.ros.org/melodic/Installation/Ubuntu

## 2.) Install our favorite text editor 

```
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
```

## 3.) Clone this repo

```
cd ~
git clone https://github.com/Texas-Aerial-Robotics/cv_tutorial.git
```

## 4.) Create our first cpp program

```
cd ~/cv_tutorial
touch cannyVideo.cpp
subl cannyVideo.cpp
```



### a.) Add defines

Now that we have a blank file in sublime open we will begin creating the program. First we have to add the opencv library so we can use the opencv functions and structures 

```
#include <opencv2/opencv.hpp>

```

### b.) Create our main 

We will be creating our main function. This is where the program will start running our operations line by line

```
int main()
{


	return 0;
}
```
return 0 specifies the end of the program. all of the code we write from now on will be inbetween `main(){ ` and `return 0;`

### c.) Open the camera stream 

```
cv::VideoCapture cap(0);
if(!cap.isOpened())  // check if we are able to open the camera stream 
{
    return -1;
}
```
`cv::VideoCapture` is an opencv object that gives access to our camera. The following if statement makes sure the computer successfully pens the stream before going on 

### d.) Define variables to hold our images 

```
cv::Mat src, gray, edges;
```

`cv::Mat` is an object that we can use store our images 

### e.) Write while loop
```
while(cap.isOpened())
{
    cap.read(src); // get a new frame from camera
    cv::imshow("source", src);
    cv::cvtColor( src, gray, cv::COLOR_BGR2GRAY );
    cv::Canny( gray, edges, 50, 150, 3 );
    cv::imshow("edges", edges);
    cv::waitKey(30);
}
```
This block of code will run indefinetly.
`cap.read(src)`  will take the current image in the stream and assign it the Mat src.  
`cv::imshow("source", src)` will display the current image our camera sees

#### Simple image processing 

this line of code will take the color image and convert it to grayscale
`cv::cvtColor( src, gray, cv::COLOR_BGR2GRAY );`
this line will detect edges in the image. Listen to my in depth explaination 
`cv::Canny( gray, edges, 50, 150, 3 );`
format (input image, output image, min gradient threshold, max gradient threshold, mask size) 

##### threshold value explaination 
```
a.) if a pixel gradient is higher than the upper threshold, the pixel is accepted as an edge
b.) If a pixel gradient value is below the lower threshold, then it is rejected.
c.) If the pixel gradient is between the two thresholds, then it will be accepted only if it is connected to a pixel that is above the upper threshold.
```

finally ` cv::waitKey(30);` waits 30 ms for the gui to update.

## 5.) compiling 

In terminal  
### create CMakeLists 
```
cd ~/cv_tutorial/
subl CMakeLists.txt 
```
paste the following code
```
cmake_minimum_required(VERSION 3.5 )
set(CMAKE_CXX_STANDARD 11)
project( video )
find_package( OpenCV REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIRS} )
add_executable( video cannyVideo.cpp )
target_link_libraries( video ${OpenCV_LIBS})
```
ctl-s to save 

in terminal
```
mkdir build 
cd build
cmake ..
make 
```

## 6.) 
run 
```
./video
```

WE DID IT!

## 7.) more reading

canny explaination - https://docs.opencv.org/master/da/d5c/tutorial_canny_detector.html
opencv tutorials - https://docs.opencv.org/master/d9/df8/tutorial_root.html


## Full code 

```
#include <opencv2/opencv.hpp>

int main()
{
	cv::VideoCapture cap(0);
    if(!cap.isOpened())  // check if we are able to open the camera stream
        return -1;
    //define image objects 
    cv::Mat src, gray, edges;
    while(cap.isOpened())
    {
	    
	    cap.read(src); // get a new frame from camera
	    cv::cvtColor( src, gray, cv::COLOR_BGR2GRAY );
	    cv::imshow("source", src);
	    cv::Canny( gray, edges, 50, 150, 3 );
	    cv::imshow("edges", edges);
	    cv::waitKey(30);
	}
	return 0;
}
```


