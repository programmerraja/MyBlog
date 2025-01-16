---
title: "Sherlock Holmes : The Mystery of the Erratic Logstash"
date: 2024-09-03T05:57:21.2121+05:30
draft: false
tags:
  - Sherlock_holmes
  - logstash
---
**Welcome to our Sherlock Holmes-inspired Tech Adventure Series!**  
Imagine each technical challenge as a thrilling mystery waiting to be unraveled. Like Sherlock Holmes with his sharp eye for detail, I’ll tackle each problem with wit and precision. Let’s dive in and solve these cases together!

### The Case: Logstash Unexpectedly Stopping

If you're familiar with the ELK stack—Elasticsearch, Logstash, and Kibana—you know it’s a powerful trio for managing and visualizing log data. Logstash, a crucial player in this stack, processes and forwards logs to Elasticsearch.

Recently, we encountered a puzzling issue with Logstash after migrating it from an old virtual machine (VM) to a new one. We noticed a concerning pattern: Logstash stopped at least once per day, triggering alerts and requiring manual restarts. This issue seemed to have appeared immediately following the VM migration.

### Initial Investigation: What Was Causing the Shutdown?

Our first step was to investigate why Logstash was stopping. Checking the logs, we found the following error message: `ERROR - Received SIGTERM. Terminating process`. This error indicated that Logstash was receiving a SIGTERM signal, a standard signal used to request program termination.

We initially suspected high memory or CPU usage might be the cause, so we examined the system metrics. However, everything appeared normal.

### Discovering the True Culprit: Automatic VM Updates

Our next clue came from reviewing the cloud activity logs. We observed a pattern: Logstash stopped exactly when a security update was applied to the VM. This led to the realization that the VM itself was being restarted as part of the update process, which caused Logstash to stop.

### The Solution: Ensuring Logstash's Resilience

To resolve this issue, we needed to ensure that Logstash would restart automatically whenever the VM did. We accomplished this by adding Logstash to the systemd service manager, as outlined below:

- **Create a Systemd Service File for Logstash**: We created a service file for Logstash at `/etc/systemd/system/logstash.service`, which includes configuration settings to manage Logstash as a system service.
    
- **Reload Systemd and Enable the Service**: We reloaded the systemd configuration and enabled the Logstash service to start automatically on boot.
    
- **Start the Logstash Service**: Finally, we started the Logstash service using systemctl.
    

```
[Unit]  
Description=Logstash Service  
After=network.target  

[Service]  
Type=simple  
User=logstash  
Group=logstash  
ExecStart=/usr/share/logstash/bin/logstash --path.settings /etc/logstash  
Restart=always  
RestartSec=5  

[Install]  
WantedBy=multi-user.target
```

By doing this, we ensured Logstash would automatically start after VM restarts, eliminating the need for manual intervention.

Stay tuned for our next adventure, where we continue to unravel the mysteries of the infrastructure world, one case at a time. Until then, keep your magnifying glasses ready and your curiosity sharp.

**If this article was helpful, please clap 👏 and follow. Thank you!**