+++
title = 'Automate, Relax, Repeat: The Power of Coding (#2)'
date = 2024-08-17T04:35:58.5858+05:30
draft = false
tags =["automation","andriod","adb","python"]
+++ 

Hello, fellow techies! I am super stoked to introduce you to my new blog series where I'll be sharing my journey of automating everything boring using code. Being a guy on the lookout for how to live life easily and simply most of the time, I have come to realize that programming is just the correct tool to do well—and anything monotonous, freeing some more time for the things that count.

This series is about sharing my experiences, guides, and tips while automating activities from easy to complex in daily activities. So if you're ready to take the Automation Challenge with me, then let's get started!

## **Automating YouTube Shorts for My Channel**

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

I hope this blog series inspires you to explore automation in your own projects. Stay tuned for more updates as I continue to automate and optimize various aspects of my digital life. If you have any questions or suggestions, feel free to leave a comment!

**Check out the code on** [GitHub](https://github.com/programmerraja/YouTube-video-automation) 









