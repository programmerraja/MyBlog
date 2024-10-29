---
title: "How I Hacked a Company’s Recruitment: Test The Unexpected Tech Adventure of My College Life"
date: 2024-08-04T10:01:36.3636+05:30
draft: false
tags:
  - hacking
---
Ah, college life! The thrill of final year comes with the excitement of job placements. We all know the drill: companies come to campus, conduct aptitude tests, coding challenges, and sometimes, we get to showcase our skills in a high-stakes interview. But what if I told you that one of those tests turned into an unexpected adventure involving a bit of hacking? Buckle up as I share how I turned a routine exam into an impromptu tech experiment—and how it all unfolded.

### The Recruitment Challenge

In our final year, our college arranged several companies to visit for recruitment. Most of them conducted their tests on paper, but one company decided to go digital. They sent us an online exam link with login credentials. The test consisted of multiple-choice questions (MCQs) and coding problems. 

As usual, we were all set to tackle the test. However, my curiosity got the better of me. Instead of focusing solely on the questions, I started exploring the technical side of the exam. I wondered how the website was fetching questions and submitting answers.

### The Discovery

While everyone else was busy answering questions, I began to investigate the website. I noticed a pattern in the URLs used to fetch the questions: `http://companyDomain.in/ConductTestApplication/test/getQuestions/[1-100]`. The numbers in the URL represented question numbers, and the server responded with the questions in JSON format.

Intrigued, I realized there was no authentication required to access this endpoint. So, I decided to test it out. I copied the URL and opened it in incognito mode. Lo and behold, I got a response with all the questions.

### The Script

After the exam, back at home, I couldn’t resist the urge to see where this would lead. I wrote a script to fetch all the questions from the website. With the questions in hand, I created a static webpage displaying them and hosted it online. I shared the link with a few friends just for fun.

### The Ethical Dilemma

Looking back, I realize that what I did wasn’t the most ethical thing. Hacking into a test and sharing it with friends wasn’t exactly in line with good conduct. But, I have to admit, it was an exhilarating experience that taught me a lot about web security and curiosity-driven problem solving.

### Conclusion

So, there you have it how a routine college recruitment test turned into a hacking adventure. It was a mix of curiosity, technical skill, and a dash of mischief. While I don’t endorse unethical practices, this experience was a memorable chapter of my college life and a valuable lesson in the world of cybersecurity.