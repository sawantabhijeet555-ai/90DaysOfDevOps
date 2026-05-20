# Linux Practice Note

## Process Checks

# Command 1 - View Running Processes 

ubuntu@ip-172-31-13-255:~$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.8  1.5  24840 15540 ?        Ss   08:45   0:01 /sbin/init
root           2  0.0  0.0      0     0 ?        S    08:45   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    08:45   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   08:45   0:00 [kworker/R-rcu_gp]
root           5  0.0  0.0      0     0 ?        I<   08:45   0:00 [kworker/R-sync_wq]
root           6  0.0  0.0      0     0 ?        I<   08:45   0:00 [kworker/R-kvfree_rcu_reclaim]
root           7  0.0  0.0      0     0 ?        I<   08:45   0:00 [kworker/R-slub_flushwq]
root           8  0.0  0.0      0     0 ?        I<   08:45   0:00 [kworker/R-netns]
root           9  0.0  0.0      0     0 ?        I    08:45   0:00 [kworker/0:0-events]
root          10  0.0  0.0      0     0 ?        I<   08:45   0:00 [kworker/0:0H-kblockd]
root          11  0.0  0.0      0     0 ?        I    08:45   0:00 [kworker/0:1-events]
root          12  0.0  0.0      0     0 ?        I    08:45   0:00 [kworker/u60:0-events_power_efficient]
root          13  0.0  0.0      0     0 ?        I<   08:45   0:00 [kworker/R-mm_percpu_wq]
root          14  0.0  0.0      0     0 ?        S    08:45   0:00 [ksoftirqd/0]

# Command 2- Live Process Monitoring
ubuntu@ip-172-31-13-255:~$ top

# Command 3 - Find SSH Process
ubuntu@ip-172-31-13-255:~$ pgrep ssh
862
885
1448

# Command 4 Process Tree

ubuntu@ip-172-31-13-255:~$ pstree
systemd─┬─ModemManager───3*[{ModemManager}]
        ├─acpid
        ├─2*[agetty]
        ├─amazon-cloudwat───7*[{amazon-cloudwat}]
        ├─amazon-ssm-agen─┬─ssm-agent-worke───11*[{ssm-agent-worke}]
        │                 └─7*[{amazon-ssm-agen}]
        ├─chronyd-starter───chronyd───chronyd
        ├─cron
        ├─dbus-daemon
        ├─multipathd───7*[{multipathd}]
        ├─networkd-dispat
        ├─polkitd───3*[{polkitd}]
        ├─rsyslogd───3*[{rsyslogd}]
        ├─snapd───7*[{snapd}]
        ├─sshd───sshd-session───sshd-session───bash─┬─pstree
        │                                           └─vim───{vim}
        ├─systemd───(sd-pam)
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-network
        ├─systemd-resolve
        ├─systemd-udevd
        ├─udisksd───6*[{udisksd}]
        └─unattended-upgr───{unattended-upgr}

# Command 5 - Find Process ID

ubuntu@ip-172-31-13-255:~$ pidof systemd

---------------------------------------------------------------------------------------------------------------------------
## Service Check Section

# Command 6 - Check SSH Service Status

ubuntu@ip-172-31-13-255:~$ systemctl status ssh

# command 7 -List Running Services 

ubuntu@ip-172-31-13-255:~$ systemctl list-units --type=service --state=running
  UNIT                                           LOAD   ACTIVE SUB     DESCRIPTION                                                   
  acpid.service                                  loaded active running ACPI event daemon
  amazon-cloudwatch-agent.service                loaded active running Amazon CloudWatch Agent
  chrony.service                                 loaded active running chrony, an NTP client/server
  cron.service                                   loaded active running Regular background program processing daemon
  dbus.service                                   loaded active running D-Bus System Message Bus
  getty@tty1.service                             loaded active running Getty on tty1
  ModemManager.service                           loaded active running Modem Manager
  multipathd.service                             loaded active running Device-Mapper Multipath Device Controller
  networkd-dispatcher.service                    loaded active running Dispatcher daemon for systemd-networkd
  polkit.service                                 loaded active running Authorization Manager
  rsyslog.service                                loaded active running System Logging Service
  serial-getty@ttyS0.service                     loaded active running Serial Getty on ttyS0
  snap.amazon-ssm-agent.amazon-ssm-agent.service loaded active running Service for snap application amazon-ssm-agent.amazon-ssm-agent
  snapd.service                                  loaded active running Snap Daemon
  ssh.service                                    loaded active running OpenBSD Secure Shell server
  systemd-journald.service                       loaded active running Journal Service
  systemd-logind.service                         loaded active running User Login Management
  systemd-networkd.service                       loaded active running Network Management
  systemd-resolved.service                       loaded active running Network Name Resolution
  systemd-udevd.service                          loaded active running Rule-based Manager for Device Events and Files
  udisks2.service                                loaded active running Disk Manager
  unattended-upgrades.service                    loaded active running Unattended Upgrades Shutdown
  user@1000.service                              loaded active running User Manager for UID 1000

Legend: LOAD   → Reflects whether the unit definition was properly loaded.
        ACTIVE → The high-level unit activation state, i.e. generalization of SUB.
        SUB    → The low-level unit activation state, values depend on unit type.

23 loaded units listed .

# Command 8 - Check if SSH is Active

ubuntu@ip-172-31-13-255:~$ systemctl is-active ssh
active 

# command 9 - Restart SSH Service 

ubuntu@ip-172-31-13-255:~$ sudo systemctl restart ssh


---------------------------------------------------------------------------------------------------------------------------
## Log Checks Section

# Command 10 - View SSH Logs

ubuntu@ip-172-31-13-255:~$ journalctl -u ssh
May 18 06:10:32 ip-172-31-13-255 systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
May 18 06:10:32 ip-172-31-13-255 sshd[994]: Server listening on 0.0.0.0 port 22.
May 18 06:10:32 ip-172-31-13-255 systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
May 18 06:10:32 ip-172-31-13-255 sshd[994]: Server listening on :: port 22.
May 18 06:14:05 ip-172-31-13-255 ec2-instance-connect[1228]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:2DwI8bBRHTBQFkjNINmeoSuv3nPy5eHLcqNgT4ZPbAg
May 18 06:14:05 ip-172-31-13-255 ec2-instance-connect[1377]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:2DwI8bBRHTBQFkjNINmeoSuv3nPy5eHLcqNgT4ZPbAg
May 18 06:14:05 ip-172-31-13-255 ec2-instance-connect[1409]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:2DwI8bBRHTBQFkjNINmeoSuv3nPy5eHLcqNgT4ZPbAg, >
May 18 06:14:06 ip-172-31-13-255 sshd-session[1119]: Accepted publickey for ubuntu from 13.233.177.5 port 60412 ssh2: ED25519 SHA256:2DwI8bBRHTBQFkjNINmeoSuv3nPy5eHLcqNgT4ZPbAg
May 18 06:14:06 ip-172-31-13-255 sshd-session[1119]: pam_unix(sshd:session): session opened for user ubuntu(uid=1000) by ubuntu(uid=0)
May 18 06:16:25 ip-172-31-13-255 sshd-session[1742]: Connection closed by 43.134.182.227 port 43580
May 18 06:41:13 ip-172-31-13-255 sshd-session[1825]: Connection closed by authenticating user root 47.115.37.164 port 41150 [preauth]
May 18 06:58:14 ip-172-31-13-255 sshd-session[1889]: Connection closed by 13.233.177.3 port 37106 [preauth]
May 18 06:58:27 ip-172-31-13-255 ec2-instance-connect[2016]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:ZdX0yW+0Z+UcPSRvw4aq0Fk9ZWQnbsMBTrH2Me/WxH8
May 18 06:58:27 ip-172-31-13-255 ec2-instance-connect[2048]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:ZdX0yW+0Z+UcPSRvw4aq0Fk9ZWQnbsMBTrH2Me/WxH8, >
May 18 06:58:27 ip-172-31-13-255 ec2-instance-connect[2195]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:ZdX0yW+0Z+UcPSRvw4aq0Fk9ZWQnbsMBTrH2Me/WxH8
May 18 06:58:28 ip-172-31-13-255 ec2-instance-connect[2227]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:ZdX0yW+0Z+UcPSRvw4aq0Fk9ZWQnbsMBTrH2Me/WxH8, >
May 18 06:58:28 ip-172-31-13-255 sshd-session[1907]: Accepted publickey for ubuntu from 13.233.177.5 port 54730 ssh2: ED25519 SHA256:ZdX0yW+0Z+UcPSRvw4aq0Fk9ZWQnbsMBTrH2Me/WxH8
May 18 06:58:28 ip-172-31-13-255 sshd-session[1907]: pam_unix(sshd:session): session opened for user ubuntu(uid=1000) by ubuntu(uid=0)
May 18 07:09:44 ip-172-31-13-255 ec2-instance-connect[3418]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:MlfInODlw0t5VKbujbEzSevU+CcoBjIhK6HwjonbEAA
May 18 07:09:44 ip-172-31-13-255 ec2-instance-connect[3450]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:MlfInODlw0t5VKbujbEzSevU+CcoBjIhK6HwjonbEAA, >
May 18 07:09:45 ip-172-31-13-255 ec2-instance-connect[3567]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:MlfInODlw0t5VKbujbEzSevU+CcoBjIhK6HwjonbEAA
May 18 07:09:45 ip-172-31-13-255 ec2-instance-connect[3599]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:MlfInODlw0t5VKbujbEzSevU+CcoBjIhK6HwjonbEAA, >
May 18 07:09:45 ip-172-31-13-255 sshd-session[3309]: Accepted publickey for ubuntu from 13.233.177.4 port 24361 ssh2: ED25519 SHA256:MlfInODlw0t5VKbujbEzSevU+CcoBjIhK6HwjonbEAA
May 18 07:09:45 ip-172-31-13-255 sshd-session[3309]: pam_unix(sshd:session): session opened for user ubuntu(uid=1000) by ubuntu(uid=0)
May 18 07:14:56 ip-172-31-13-255 sshd-session[3997]: Connection closed by authenticating user root 43.134.182.227 port 36710 [preauth]
May 18 07:14:57 ip-172-31-13-255 sshd-session[3999]: Connection closed by authenticating user root 43.134.182.227 port 36718 [preauth]
May 18 07:14:57 ip-172-31-13-255 sshd-session[4001]: Invalid user ftpuser from 43.134.182.227 port 36724
May 18 07:14:57 ip-172-31-13-255 sshd-session[4001]: Connection closed by invalid user ftpuser 43.134.182.227 port 36724 [preauth]
May 18 07:14:58 ip-172-31-13-255 sshd-session[4003]: Invalid user user from 43.134.182.227 port 36730
May 18 07:14:58 ip-172-31-13-255 sshd-session[4003]: Connection closed by invalid user user 43.134.182.227 port 36730 [preauth]
May 18 07:14:58 ip-172-31-13-255 sshd-session[4005]: Invalid user dev from 43.134.182.227 port 36734
May 18 07:14:58 ip-172-31-13-255 sshd-session[4005]: Connection closed by invalid user dev 43.134.182.227 port 36734 [preauth]
May 18 07:14:58 ip-172-31-13-255 sshd-session[4007]: Invalid user git from 43.134.182.227 port 36750
May 18 07:14:59 ip-172-31-13-255 sshd-session[4007]: Connection closed by invalid user git 43.134.182.227 port 36750 [preauth]
lines 1-34

# Command 11 - View Recent logs 
ubuntu@ip-172-31-13-255:~$ journalctl -n 20
May 20 09:09:02 ip-172-31-13-255 systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
May 20 09:09:02 ip-172-31-13-255 sudo[2117]: pam_unix(sudo:session): session closed for user root
May 20 09:09:27 ip-172-31-13-255 sudo[2138]: pam_unix(sudo:session): session opened for user root(uid=0) by ubuntu(uid=1000)
May 20 09:09:27 ip-172-31-13-255 sudo[2138]: ubuntu : TTY=/dev/pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/systemctl restart sssh
May 20 09:09:27 ip-172-31-13-255 sudo[2138]: pam_unix(sudo:session): session closed for user root
May 20 09:09:33 ip-172-31-13-255 sudo[2152]: pam_unix(sudo:session): session opened for user root(uid=0) by ubuntu(uid=1000)
May 20 09:09:33 ip-172-31-13-255 sudo[2152]: ubuntu : TTY=/dev/pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/systemctl restart ssh
May 20 09:09:33 ip-172-31-13-255 sshd[2126]: Received signal 15; terminating.
May 20 09:09:33 ip-172-31-13-255 systemd[1]: Stopping ssh.service - OpenBSD Secure Shell server...
May 20 09:09:33 ip-172-31-13-255 systemd[1]: ssh.service: Deactivated successfully.
May 20 09:09:33 ip-172-31-13-255 systemd[1]: Stopped ssh.service - OpenBSD Secure Shell server.
May 20 09:09:33 ip-172-31-13-255 systemd[1]: sshd-keygen.service - Generate sshd host keys on first boot skipped, unmet condition check ConditionFirstBoot=yes
May 20 09:09:33 ip-172-31-13-255 systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
May 20 09:09:33 ip-172-31-13-255 sshd[2161]: Server listening on 0.0.0.0 port 22.
May 20 09:09:33 ip-172-31-13-255 sshd[2161]: Server listening on :: port 22.
May 20 09:09:33 ip-172-31-13-255 systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
May 20 09:09:33 ip-172-31-13-255 sudo[2152]: pam_unix(sudo:session): session closed for user root
May 20 09:10:14 ip-172-31-13-255 systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
May 20 09:10:14 ip-172-31-13-255 systemd[1]: sysstat-collect.service: Deactivated successfully.
May 20 09:10:14 ip-172-31-13-255 systemd[1]: Finished sysstat-collect.service - system activity accounting tool.

# Command 12 - View System Logs

ubuntu@ip-172-31-13-255:~$ tail -n 50 /var/log/syslog

# Command 13 - Kernel Logs

ubuntu@ip-172-31-13-255:~$ sudo dmesg | tail -n 20

# Command 14 -Real-Time Log Monitoring 

ubuntu@ip-172-31-13-255:~$ journalctl -f
---------------------------------------------------------------------------------------------------------------------------
## System Information 

# Command 15 - Disk Usage

ubuntu@ip-172-31-13-255:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       6.7G  2.8G  3.9G  42% /
tmpfs           476M     0  476M   0% /dev/shm
tmpfs           191M  896K  190M   1% /run
tmpfs           476M     0  476M   0% /tmp
none            1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
none            1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
/dev/xvda13     989M   95M  827M  11% /boot
/dev/xvda15     105M  6.3M   99M   7% /boot/efi
none            1.0M     0  1.0M   0% /run/credentials/systemd-networkd.service
none            1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
none            1.0M     0  1.0M   0% /run/credentials/serial-getty@ttyS0.service
tmpfs            96M  8.0K   96M   1% /run/user/1000

# Command 16 - Memory Usage 

ubuntu@ip-172-31-13-255:~$ free -m
               total        used        free      shared  buff/cache   available
Mem:             951         362         177           1         527         589
Swap:              0           0           0

# Command 17 - System Uptime

ubuntu@ip-172-31-13-255:~$ uptime
 09:21:54 up 36 min,  1 user,  load average: 0.11, 0.10, 0.09

# Command 18 - Network Information

ubuntu@ip-172-31-13-255:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enX0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc fq_codel state UP group default qlen 1000
    link/ether 0a:1a:ff:9a:99:b3 brd ff:ff:ff:ff:ff:ff
    altname enx0a1aff9a99b3
    inet 172.31.13.255/20 metric 100 brd 172.31.15.255 scope global dynamic enX0
       valid_lft 3191sec preferred_lft 3191sec
    inet6 fe80::81a:ffff:fe9a:99b3/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever

---------------------------------------------------------------------------------------------------------------------------
## Troubleshooting Section

# Problem : SSH Service is not Responding 

# Step 1:- Check SSH Service Status

  204  systemctl status ssh

# Step 2 :- Check SSH Logs

  205  journalctl -u ssh

# step 3 :- Verify SSh Process 

  206  pgrep ssh

# Step 4 :- Restart SSH Service

  207  sudo systemctl restart ssh

# Step 5 :- Confirm Service is Active 

  208  systemctl is-active ssh