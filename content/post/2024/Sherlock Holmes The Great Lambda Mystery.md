---
title: "Sherlock Holmes: The Great Lambda Mystery"
date: 2024-07-02T12:27:32.3232+05:30
tags:
  - lambda
  - sherlock
  - holmes
  - npm
  - Sherlock_holmes
---

Welcome to our Sherlock Holmes-inspired tech adventure Series! Imagine each technical challenge as a thrilling mystery waiting to be solved. Like Sherlock Holmes with his sharp eye for detail, I'll tackle the problem with wit and precision. Let's dive in and crack these cases together!

#### The Early Morning Disturbance

It was 4 o'clock in the morning when I received an urgent call from a colleague. Our Lambda function had suddenly stopped working. Groggy but alert, I sprang into action, ready to tackle the issue head-on. The error logs pointed me to a peculiar message: `npm ERR! rofs EROFS: read-only file system, mkdir '/home/sbx_user1051'`.

#### The First Clue: Read-Only File System

The error message was clear: npm was attempting to write to a read-only file system. But why was this happening all of a sudden? My first step was to examine our recent code changes. Had we inadvertently introduced code that attempted to write to the disk? A thorough review revealed no such changes.

#### The Google Search: A Trail of Breadcrumbs

With no immediate answers in the code, I turned to the vast expanse of the internet. After some diligent searching, I stumbled upon a Stack Overflow answer suggesting to set the environment variable `NPM_CONFIG_CACHE=/tmp/.npm`. Intrigued, I applied the suggested fix. To my relief, it worked. The Lambda function sprang back to life, and I could finally breathe easy.

#### The Curious Case of the Environment Variable

But why did this environment variable fix the issue? My curiosity was piqued, and I spent the next two hours delving into the depths of npm documentation and GitHub repositories. I discovered that starting with npm 8, a change in the `@npmcli/run-script` module caused scripts to be written into the temporary directory (`tmpdir()`) of the user path (`/home/sbx_user1051`). This information was confirmed in the npm CI repository's [issue tab](https://github.com/npm/cli/issues/7105)

#### The Final Piece of the Puzzle

We now understood how setting `NPM_CONFIG_CACHE=/tmp/.npm` resolved the issue. However, one question remained: why did this problem arise so suddenly? More research revealed the answer. AWS Lambda had deprecated Node.js 14 and automatically upgraded our environment to the latest version, which included npm 8. This upgrade introduced the change that caused our issue.


Stay tuned for our next adventure, where we continue to unravel the mysteries of the infrastructure world, one case at a time. Until then, keep your magnifying glasses handy and your curiosity alive.