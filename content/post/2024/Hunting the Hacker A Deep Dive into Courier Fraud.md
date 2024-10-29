---
title: "Hunting the Hacker: A Deep Dive into Courier Fraud"
date: 2024-07-20T07:26:19.1919+05:30
draft: false
tags:
  - hacking
  - scam
---


In today's digital age, cyber scams are becoming increasingly sophisticated. Recently, one of our colleagues received a suspicious call that led to an attempted scam. This blog post details our investigation into the scam, revealing the techniques used by the fraudsters and how we managed to thwart their efforts.

### The Call

It all began when one of our colleagues received a phone call from someone claiming to be from a courier company. They were told that our college had an courier to be delivered, but to complete the delivery, a payment of ₹6 was required. The caller insisted on sending an APK file via WhatsApp, which needed to be installed to make the payment.

### The APK

Suspicious of the request, our colleague forwarded the APK to me, knowing my expertise in software development and cybersecurity. My investigation began by downloading and extracting the contents of the APK. Inside, I found several dex files, which I knew contained the app's compiled source code. Using a tool called [jadx](https://github.com/skylot/jadx), (`jadx -d outputDirName extractedapkfile`) I decompiled the APK to inspect its source code.

### The Investigation

As I delved into the code, I searched for URLs that might indicate where the app was communicating. I discovered a domain:`parceltracing.com.` Visiting the website, I found it appeared to be a legitimate parcel tracking service with a professional landing page.

Continuing my investigation, I found a suspicious line of code: `this.webView.loadUrl("file:///android_asset/index.html");`. This indicated that the app was loading a local HTML file. I examined the contents of `index.html` and found two images of parcels and a form requesting an address and payment amount. Clicking 'Next' on the form led to another page asking for UPI payment details, including phone number, bank name, and UPI ID.

### The Scam Unveiled

The final piece of the puzzle was found in the source code of the payment page. It showed that the form was sending a payload containing the UPI ID and other details to a server at `https://sonuv.parceltracing.com/%24Gh3_%402%26E*(b%5E%409%23L6K%40/success.php`. This was how the scammers were attempting to steal UPI IDs and potentially siphon money from unsuspecting victims.

### The Counterattack

Armed with this information, I decided to take action to protect others from falling victim to this scam. I wrote a script to flood the scammer's server with random names, UPI IDs, and other details, effectively filling their database with junk data. This not only rendered their current scam attempt useless but also disrupted their operations, preventing them from cheating others.
