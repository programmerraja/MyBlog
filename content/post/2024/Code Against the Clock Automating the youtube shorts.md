+++
title = 'Code Against the Clock: Automating the youtube shorts creation'
date = 2024-08-17T04:35:58.5858+05:30
draft = false
tags =["automation","andriod","adb","python",'code aganist the clock']
+++ 


Welcome back to "Code Against the Clock!" blog series where I’ll reveal how I turned my most boring tasks into streamlined, time-saving machines. I’ll share the exact steps I took to automate these chores and the cool tricks I discovered along the way. Ready to see how you can save time and make life a bit more exciting? Let’s dive in and get your tasks on autopilot!

## **A Solution to Balancing Content Creation with Busy Schedules**

Today, I'll be sharing how I automated YouTube shorts for my channel. I've been running my YouTube channel for over two years, actively posting videos about teaching programming languages during my college days. However, when I entered my final year, I found it challenging to dedicate time to posting videos regularly. I didn't want my channel to stagnate, so I decided to automate video creation.

**The Idea**

My initial idea was to create videos featuring code writing with background music, requiring minimal human intervention. Then, I thought, why not create animations or cartoons using HTML and CSS, and add background music? You can check out one of my videos [here](https://www.youtube.com/watch?v=wvIRKYiMALo) which make use of automation.

**The Challenge**

I initially planned to use Python's [PyAutoGUI](https://pypi.org/project/PyAutoGUI/) library in laptop, but I realized it would be more challenging than I anticipated. So, I decided to learn about automating Android apps and discovered the Android Debug Bridge (ADB), a versatile command-line tool that lets you communicate with a device.

## **Breaking Down the Problem**

I chose to work with [ADB](https://developer.android.com/tools/adb) and found a Python library [ppadb](https://pypi.org/project/pure-python-adb/)  that helps interact with it. I came up with an algorithm that would guide my script's logic:

**The Algorithm**

**Step 1: Initialize Android Device Connection**

To start, I need to establish a connection to the Android device using the `ppadb` library. This will allow me to set up the device for automation and perform subsequent actions.

**Step 2: Write HTML Code**

Next, I need to write HTML code to the Android device's web page editor. I'll create a string containing the HTML code and use the `writeHTML` function to send it to the device.

**Step 3: Write CSS Code**

After that, I need to write CSS code to the Android device's CSS editor. I'll split the CSS code into individual styles using the `splitme` separator and write each style to the device using the `writeCss` function.

**Step 4: Write JavaScript Code**

Then, I need to write JavaScript code to the Android device's JavaScript editor. I'll create a string containing the JavaScript code and use the `writeJs` function to send it to the device (although this functionality is not implemented in this code).

**Step 5: Start Video Recording**

Next, I need to start a video recording on the Android device using the `videoStart` function. This will capture the device's screen activity.

**Step 6: Wait for Animations to Complete**

After starting the video recording, I need to wait for a specified amount of time (5 seconds in this code) to allow any animations to complete.

**Step 7: Stop Video Recording**

Once the animations have completed, I need to stop the video recording on the Android device using the `videoStop` function.

**Step 8: Retrieve Video from Android Device**

Then, I need to retrieve the recorded video from the Android device using the `getVideoToSys` function.

**Step 9: Process Video**

After retrieving the video, I need to process it using the `editVideo` function which will run a javascript program as subprocess in python and the javascript use `fluent-ffmpeg` lib to add backgrround music to the video

**Step 10: Upload Video to Android Device**

Finally, I need to upload the processed video back to the Android device using the `putVideoToMobile` function.


After following the steps outlined in the algorithm, I was able to automate the creation of YouTube Shorts with impressive results. The automation process not only streamlined content creation but also ensured a consistent quality across videos. The background music and animations added a professional touch, making the content engaging and visually appealing.

Note: If you want the source code feel free to ping me :)
### **Your Turn!**

Have you ever automated a task using code? Share your experiences and tips in the comments below! What tasks do you wish you could automate? Let's discuss!

Thanks for joining me on this automation journey. Don’t forget to subscribe to my blog for more tips and updates. Happy coding!











