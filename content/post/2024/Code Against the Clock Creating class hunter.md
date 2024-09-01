+++
title = 'Code Against the Clock: Creating the class hunter'
date = 2024-08-16T20:17:08.088+05:30
draft = false
tags =["automation","nodejs","Puppeteer",'code aganist the clock']
+++ 


Hey there! Welcome to "**Code Against the Clock,"** blog series where I’ll reveal how I turned my most boring tasks into streamlined, time-saving machines. I’ll share the exact steps I took to automate these chores and the cool tricks I discovered along the way. Ready to see how you can save time and make life a bit more exciting? Let’s dive in and get your tasks on autopilot!

### **The COVID-19 Pandemic: A Turning Point**

Who can forget the sudden halt in March 2020 when the COVID-19 pandemic brought the world to a standstill? Schools and colleges closed, and virtual classes became the new norm. As a college student, I was expected to attend online classes, but I had other commitments that couldn't be ignored. My family needed my assistance during the lockdown, and spending hours staring at a computer screen wasn't an option.

That's when I decided to take matters into my own hands. I had recently started learning Node.js, and I saw an opportunity to use my newfound skills to automate my virtual classes. Why not, right? I was already spending hours writing code, so why not write it in a way that would do all the class attendance for me?

### **Discovering Puppeteer**

During my research, I stumbled upon Puppeteer, a powerful tool that can be used with Node.js to automate browser functions. It was the perfect solution for my problem, as I could create a script that would launch a browser, log into my online classroom portal, and attend classes on my behalf.

### **Breaking Down the Problem**

Before I started coding, I needed to break down the problem into smaller, manageable tasks. I came up with an algorithm that would guide my script's logic:

**The Algorithm**

1. **Find Present Class**: Identify the current class schedule and find the next class to attend.
2. **Start Browser**: Launch a new browser instance using Puppeteer.
3. **Login**: Determine the login method: use stored cookies to authenticate or sign in with Google.
4. **Open Class**: Navigate to the online class platform and join the class.
5. **Check Number of Students**: Verify the number of students in the class. If the student count is greater than a configured count, join the class.
6. **Leave Class (if necessary)**: If there are no students in the class, leave the class.
7. **Find Next Class Time and Join**: Schedule the next class attendance based on the class schedule.
8. **Restart**: Repeat the process for the next class.

**The Result**

It took me just a day to complete the project, and I started using it for my online classes. The feeling of automating a tedious task was incredibly empowering!

You can find the code [here](https://github.com/programmerraja/ClassHunter)

### **Your Turn!**

Have you ever automated a task using code? Share your experiences and tips in the comments below! What tasks do you wish you could automate? Let's discuss!

Thanks for joining me on this automation journey. Don’t forget to subscribe to my blog for more tips and updates. Happy coding!



#### **Visual Elements:**

1. **Cartoon Version of You:**
    
    - Draw yourself in a playful, cartoon style. You could be depicted sitting at a computer, surrounded by code snippets and a coffee mug. You might have an excited or thoughtful expression.
2. **Automation Theme:**
    
    - Add elements that represent automation, such as gears, code snippets, and maybe a small robotic figure or a laptop with an animated play button to symbolize automation.
3. **Puppeteer Reference:**
    
    - Incorporate a visual reference to Puppeteer, like a puppet or marionette controlling a browser window. This can help illustrate the tool you're using.
4. **Classroom Setting:**
    
    - Include a cartoon representation of an online classroom. This can be shown on the computer screen with cartoon students and a teacher.
5. **Dynamic Background:**
    
    - Use a dynamic and colorful background that adds energy and excitement. It could include coding elements like brackets and semicolons, or abstract tech-themed patterns.
6. **Text Elements:**
    
    - Add the title of your blog series at the top or bottom. Use a bold, fun font that captures the essence of your content.

### **Example Layout:**

1. **Background:** Colorful tech-themed abstract patterns.
2. **Center:** Cartoon version of you working at a computer with a browser window open showing an online class.
3. **Top:** Title "Automating Boring Tasks" in bold, fun text.
4. **Bottom:** "My Journey with Code" in a smaller, complementary font.
5. **Side Elements:** Little icons or illustrations of gears, code snippets, and a marionette/puppet.