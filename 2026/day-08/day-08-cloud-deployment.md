Abhijeet Sawant@IMSMUMDTO01182 MINGW64 ~/downloads (master)
$ find . -name cloudwatchtestvm.em

Abhijeet Sawant@IMSMUMDTO01182 MINGW64 ~/downloads (master)
$ find . -name cloudwatchtestvm.pem
./cloudwatchtestvm.pem

Abhijeet Sawant@IMSMUMDTO01182 MINGW64 ~/downloads (master)
$ chmod 400 "cloudwatchtestvm.pem"

## Part 1: Launch Cloud Instance & SSH Access (15 minutes)
# Step 1: Create a Cloud Instance

# Step 2: Connect via SSH


Abhijeet Sawant@IMSMUMDTO01182 MINGW64 ~/downloads (master)$ ssh -i "cloudwatchtestvm.pem" ubuntu@ec2-3-7-253-54.ap-south-1.compute.amazonaws.com
The authenticity of host 'ec2-3-7-253-54.ap-south-1.compute.amazonaws.com (3.7.253.54)' can't be established.
ED25519 key fingerprint is SHA256:948vjcOdxvZYnpV20MJkwSjxj6nQUsYmawxpIgLdGzE.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? Yes
Warning: Permanently added 'ec2-3-7-253-54.ap-south-1.compute.amazonaws.com' (ED25519) to the list of known hosts.
Welcome to Ubuntu 26.04 LTS (GNU/Linux 7.0.0-1004-aws x86_64)

 * Documentation:  https://docs.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Mon May 25 06:04:37 UTC 2026

  System load:  0.06              Processes:             114
  Usage of /:   30.4% of 6.61GB   Users logged in:       0
  Memory usage: 23%               IPv4 address for enX0: 172.31.14.118
  Swap usage:   0%

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

## Part 2: Install Docker & Nginx (20 minutes)
Step 1: Update System

ubuntu@ip-172-31-14-118:~$ sudo apt-get update 
Get:1 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute InRelease [136 kB]
Get:2 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute-updates InRelease [136 kB]
Get:3 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute-backports InRelease [136 kB]
Get:4 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 Packages [1483 kB]
Get:5 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute/main Translation-en [524 kB]
Get:6 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute/main amd64 Components [395 kB]
Get:7 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute/main amd64 c-n-f Metadata [32.4 kB]
Get:8 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute/universe amd64v3 Packages [16.3 MB]

ubuntu@ip-172-31-14-118:~$ sudo apt-get upgrade
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  base-files bind9-dnsutils bind9-host bind9-libs bpftool curl distro-info-data gir1.2-packagekitglib-1.0 jq libcurl3t64-gnutls libcurl4t64 libgnutls30t64 libjq1
  libnghttp2-14 libntfs-3g89t64 libpackagekit-glib2-18 linux-perf linux-tools-common motd-news-config ntfs-3g openssh-client openssh-server openssh-sftp-server packagekit
  python3-distupgrade rsync sed snapd ubuntu-release-upgrader-core vim vim-common vim-runtime vim-tiny xxd
34 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 68.4 MB of archives.
After this operation, 653 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute-updates/main amd64v3 motd-news-config all 14ubuntu6.1 [5632 B]
Get:2 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute-updates/main amd64v3 base-files amd64 14ubuntu6.1 [75.0 kB]
Get:3 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu resolute-updates/main amd64v3 sed amd64 4.9-2ubuntu1 [195 kB]

## Step 3: Install Nginx

ubuntu@ip-172-31-14-118:~$ sudo apt-get install nginx

ubuntu@ip-172-31-14-118:~$ systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Mon 2026-05-25 06:17:04 UTC; 3min 3s ago
 Invocation: 7875bcae0a1b4243aeb619e26cfdc63c
       Docs: man:nginx(8)
    Process: 12256 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 12258 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 12286 (nginx)
      Tasks: 2 (limit: 657)
     Memory: 2.3M (peak: 5M)
        CPU: 28ms
     CGroup: /system.slice/nginx.service
             ├─12286 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             └─12288 "nginx: worker process"

May 25 06:17:04 ip-172-31-14-118 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
May 25 06:17:04 ip-172-31-14-118 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.

