+++
title = 'Code Against the Clock: How I Cut Our Marketing Team’s Daily Chores with Automation'
date = 2024-08-28T07:02:01.011+05:30
draft = true
tags =['code aganist the clock']
+++ 

Welcome back to **"Code Against the Clock!"** – the blog series where I transform mundane tasks into streamlined, time-saving marvels. Today, I’m excited to share a project where I turned a repetitive, manual chore into an automated powerhouse. Ready to see how you can save time and add a touch of excitement to your workflow? Let’s dive in!

## The Backstory

Working at a startup has its perks – like the chance to collaborate with various departments and uncover inefficiencies ripe for automation. During a recent chat with our marketing team, I discovered a task that was just begging for a tech upgrade. They were manually:

- Visiting Product Hunt daily to get the top 5 products of the day.
- Gathering social media details for each product maker.
- Repeating this process every day.

As soon as I heard this, I thought, “Why not automate it?” I grabbed my laptop and started coding.

## The Problem Breakdown

To tackle this, I needed to build a scraper. Here’s a quick rundown of the approach I took:

1. **Understanding Product Hunt's Structure**: I investigated how Product Hunt renders its content and the APIs they use. They rely on server-side rendering for displaying the top products and detailed information.
    
2. **Choosing the Tools**: Since the website uses server-side rendering, I decided to use Puppeteer with Node.js. Puppeteer allows us to control a headless browser and scrape content as if we were browsing manually.
    
3. **Fetching Data**:
    - **Top 5 Products**: I started by scraping the Product Hunt website to get the top 5 products of the day.
    - **Product Details**: For each product, I clicked through to get the product ID.
    - **Maker Information**: Using the product ID, I accessed an API to fetch details about the product maker.
    - **Social Media Details**: With the maker IDs in hand, I visited each user’s profile page via Puppeteer and scraped their social media details.
    - **Data Storage**: Finally, I compiled all this information into a CSV file, making it easy for the marketing team to work with.

## Why This Matters

Automating these tasks not only saves time but also reduces human error and ensures that the marketing team always has the latest data at their fingertips. Plus, it’s a great example of how technology can streamline repetitive tasks and add value.

Note: If you want the source code feel free to ping me :)
### **Your Turn!**

Have you ever automated a task using code? Share your experiences and tips in the comments below! What tasks do you wish you could automate? Let's discuss!

Thanks for joining me on this automation journey. Don’t forget to subscribe to my blog for more tips and updates. Happy coding!