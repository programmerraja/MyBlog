---
title: "Sherlock Holmes : The Mystery of the Erratic Logstash"
date: 2024-09-03T05:57:21.2121+05:30
draft: false
tags:
  - Sherlock_holmes
  - logstash
---

Welcome to our Sherlock Holmes-inspired tech adventure Series! Imagine each technical challenge as a thrilling mystery waiting to be solved. Like Sherlock Holmes with his sharp eye for detail, I'll tackle the problem with wit and precision. Let's dive in and crack these cases together!

### The Case: Logstash Unexpectedly Stopping

If you're familiar with the ELK stack—Elasticsearch, Logstash, and Kibana—you know it’s a powerful trio for managing and visualizing log data. Logstash, a key player in this stack, processes and forwards logs to Elasticsearch.

Recently, we encountered a puzzling issue with Logstash after a we migrated the logstash from old Virtual machine to new Virtual machine. We observed a troubling pattern: Logstash stopped at least once per day. This frequent stoppage triggered alerts and required us to manually restart Logstash each time. The problem appeared to have started immediately after the VM migration.

### Initial Investigation: What Was Causing the Shutdown?

Our first step was to investigate why Logstash was stopping. Checking the logs, we found the following error message: `ERROR - Received SIGTERM. Terminating process`. This error indicated that Logstash was receiving a SIGTERM signal from the parent process. In technical terms, SIGTERM is a generic signal used to request program termination.

Suspecting that high memory or CPU usage might be the culprit, we examined the system metrics. However, everything appeared normal.

### Discovering the True Culprit: Automatic VM Updates

Our next clue came from examining the cloud activity logs. We noticed a pattern: Logstash stopped precisely when a security update was applied to the VM. This led us to the realization that the VM was being restarted as part of the update process. Consequently, Logstash was stopping because the VM itself was being restarted.

### The Solution: Ensuring Logstash's Resilience

To resolve this issue, we needed to ensure that Logstash would automatically restart whenever the VM did. We achieved this by adding Logstash to the systemctl service manager as follow.

- **Create a Systemd Service File for Logstash**: We created a service file for Logstash under `/etc/systemd/system/logstash.service`. This file contains configuration settings to manage Logstash as a system service.

- **Reload Systemd and Enable the Service**: We reloaded the systemd configuration and enabled the Logstash service to start automatically on boot.

- **Start the Logstash Service**: Finally, we started the Logstash service using systemctl.

By doing so, we configured Logstash to start automatically upon VM restarts, ensuring it would resume its operation without manual intervention.

Stay tuned for our next adventure, where we continue to unravel the mysteries of the infrastructure world, one case at a time. Until then, keep your magnifying glasses handy and your curiosity alive.

**Finally, if the article was helpful, please clap 👏and follow, thank you!**







