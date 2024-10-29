---
title: "The Joy of Debugging:  How One Tiny Typo Kept Us Busy for Hours"
date: 2024-09-29T14:25:53.5353+05:30
draft: true
tags:
  - debugging
  - meilisearch
---

Hey friends! 👋

Today, I want to share a little debugging adventure that started with what seemed like a small issue but turned into an hour-long hunt. Spoiler: it was all caused by a single typo in Meilisearch's documentation. 🙃

We’ve been using Meilisearch to enhance our product’s search functionality, and recently, we got a new requirement: We needed to trigger some business logic every time Meilisearch completed indexing.

After exploring Meilisearch’s features, we stumbled upon something that seemed like the perfect solution—**task webhooks**. This feature allows you to get notified when an indexing task is completed, exactly what we needed!

According to the [official docs](https://www.meilisearch.com/docs/learn/self_hosted/configure_meilisearch_at_launch), all we had to do was set up two environment variables: one for the webhook URL and another for the authorization header.


So, we set it up as per the documentation, started indexing some data, and patiently waited for the webhook to trigger. 🎉 It did! But... our business logic didn’t execute as expected. 😕

We checked the server logs to ensure the webhook was triggering, and it was. But something wasn’t right—our business logic still wasn't running. When we dug into the logs, we found that our server was returning a 401 status code. Uh-oh! 😟

Naturally, we thought we messed something up on our server’s side, particularly with the webhook authorization header validation. We double-checked our Meilisearch environment setup—everything looked fine. So, we decided to log the incoming request headers to see if the auth header was even being passed correctly.

To our surprise, it was completely empty. 😅

At this point, it became clear that the issue wasn’t on our side. After a lot of Googling, reading forums, and countless debugging attempts, we still couldn’t crack it.

Finally, I decided to take matters into my own hands and dive into Meilisearch’s codebase. I wanted to see exactly how they were sending the webhook event and attaching the authorization header.

And that’s when I discovered the culprit: **a simple typo in their documentation**. 🤦‍♂️

In the code, they were using the environment variable `MEILI_TASK_WEBHOOK_AUTHORIZATION_HEADER`, but the docs listed it as `MEILI_TASK_AUTHORIZATION_HEADER`. They’d accidentally left out the "WEBHOOK" part!

Once we corrected the typo in our environment variable, everything worked like a charm. 🛠️

Feeling like a true detective, I didn’t stop there. I went ahead and submitted a pull request to fix the typo in their documentation so that future developers wouldn’t have to go through the same ordeal. The PR got merged, and for a brief moment, I felt like a superhero, saving other developers' precious time. 