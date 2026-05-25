## Task 1: Create Users (20 minutes)
# Create three users with home directories and passwords:

# tokyo
# berlin
# professor


root@ip-172-31-14-118:/# sudo adduser tokyo
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for tokyo
Enter the new value, or press ENTER for the default
        Full Name []: Abhijeet
        Room Number []: 100
        Work Phone []: +91 8275628402
        Home Phone []: +91 8788493697
        Other []: 
Is the information correct? [Y/n] y
root@ip-172-31-14-118:/# cd home
root@ip-172-31-14-118:/home# ls
tokyo  ubuntu
root@ip-172-31-14-118:/home# sudo useradd -m berlin
root@ip-172-31-14-118:/home# sudo useradd -m professor
root@ip-172-31-14-118:/home# ls
berlin  professor  tokyo  ubuntu


root@ip-172-31-14-118:/# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
dhcpcd:x:996:996:DHCP Client Daemon:/usr/lib/dhcpcd:/bin/false
messagebus:x:995:995:System Message Bus:/nonexistent:/usr/sbin/nologin
syslog:x:100:101::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:989:989:systemd Resolver:/:/usr/sbin/nologin
_chrony:x:988:988:Chrony Daemon:/var/lib/chrony:/usr/sbin/nologin
tss:x:987:987:tss user for tpm2:/:/usr/sbin/nologin
uuidd:x:101:103::/run/uuidd:/usr/sbin/nologin
sshd:x:986:65534:sshd user:/run/sshd:/usr/sbin/nologin
pollinate:x:102:1::/var/cache/pollinate:/bin/false
tcpdump:x:985:985:tcpdump:/nonexistent:/usr/sbin/nologin
landscape:x:103:106::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:984:984:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
polkitd:x:983:983:User for polkitd:/:/usr/sbin/nologin
ec2-instance-connect:x:104:65534::/nonexistent:/usr/sbin/nologin
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
tokyo:x:1001:1001:Abhijeet,100,+91 8275628402,+91 8788493697:/home/tokyo:/bin/bash
berlin:x:1002:1002::/home/berlin:/bin/sh
professor:x:1003:1003::/home/professor:/bin/sh


## Task 2: Create Groups (10 minutes)
# Create two groups:
 
# developers
# admins
# Verify: Check /etc/group


ubuntu@ip-172-31-14-118:/$ sudo groupadd developers
ubuntu@ip-172-31-14-118:/$ sudo groupadd admin
groupadd: group 'admin' already exists
ubuntu@ip-172-31-14-118:/$ sudo groupadd admins
ubuntu@ip-172-31-14-118:/$ cat /etc/group
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,ubuntu
tty:x:5:
disk:x:6:
lp:x:7:
mail:x:8:
news:x:9:
uucp:x:10:
man:x:12:
proxy:x:13:
kmem:x:15:
dialout:x:20:
fax:x:21:
voice:x:22:
cdrom:x:24:ubuntu
floppy:x:25:
tape:x:26:
sudo:x:27:ubuntu
audio:x:29:
dip:x:30:ubuntu
www-data:x:33:
backup:x:34:
operator:x:37:
list:x:38:
irc:x:39:
src:x:40:
shadow:x:42:
utmp:x:43:
video:x:44:
sasl:x:45:
plugdev:x:46:
staff:x:50:
games:x:60:
users:x:100:tokyo
nogroup:x:65534:
systemd-journal:x:999:
systemd-network:x:998:
crontab:x:997:
dhcpcd:x:996:
messagebus:x:995:
syslog:x:101:
input:x:994:
sgx:x:993:
clock:x:992:
kvm:x:991:
render:x:990:
systemd-resolve:x:989:
_chrony:x:988:
tiss:x:987:
lxd:x:102:ubuntu
uuidd:x:103:
_ssh:x:104:
rdma:x:105:
tcpdump:x:985:
landscape:x:106:
fwupd-refresh:x:984:
polkitd:x:983:
admin:x:107:
netdev:x:108:
ubuntu:x:1000:
tokyo:x:1001:
berlin:x:1002:
professor:x:1003:
developers:x:1004:
admins:x:1005:


## Task 3: Assign to Groups (15 minutes)
# Assign users:

# tokyo → developers
# berlin → developers + admins (both groups)
# professor → admins
# Verify: Use appropriate command to check group membership


ubuntu@ip-172-31-14-118:/$ sudo gpasswd -a tokyo developers
Adding user tokyo to group developers

ubuntu@ip-172-31-14-118:/$ sudo usermod -aG admins berlin 
ubuntu@ip-172-31-14-118:/$ sudo usermod -aG admins professors
usermod: user 'professors' does not exist
ubuntu@ip-172-31-14-118:/$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
dhcpcd:x:996:996:DHCP Client Daemon:/usr/lib/dhcpcd:/bin/false
messagebus:x:995:995:System Message Bus:/nonexistent:/usr/sbin/nologin
syslog:x:100:101::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:989:989:systemd Resolver:/:/usr/sbin/nologin
_chrony:x:988:988:Chrony Daemon:/var/lib/chrony:/usr/sbin/nologin
tss:x:987:987:tss user for tpm2:/:/usr/sbin/nologin
uuidd:x:101:103::/run/uuidd:/usr/sbin/nologin
sshd:x:986:65534:sshd user:/run/sshd:/usr/sbin/nologin
pollinate:x:102:1::/var/cache/pollinate:/bin/false
tcpdump:x:985:985:tcpdump:/nonexistent:/usr/sbin/nologin
landscape:x:103:106::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:984:984:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
polkitd:x:983:983:User for polkitd:/:/usr/sbin/nologin
ec2-instance-connect:x:104:65534::/nonexistent:/usr/sbin/nologin
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
tokyo:x:1001:1001:Abhijeet,100,+91 8275628402,+91 8788493697:/home/tokyo:/bin/bash
berlin:x:1002:1002::/home/berlin:/bin/sh
professor:x:1003:1003::/home/professor:/bin/sh
ubuntu@ip-172-31-14-118:/$ sudo usermod -aG admins professor
ubuntu@ip-172-31-14-118:/$ cat /etc/group
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,ubuntu
tty:x:5:
disk:x:6:
lp:x:7:
mail:x:8:
news:x:9:
uucp:x:10:
man:x:12:
proxy:x:13:
kmem:x:15:
dialout:x:20:
fax:x:21:
voice:x:22:
cdrom:x:24:ubuntu
floppy:x:25:
tape:x:26:
sudo:x:27:ubuntu
audio:x:29:
dip:x:30:ubuntu
www-data:x:33:
backup:x:34:
operator:x:37:
list:x:38:
irc:x:39:
src:x:40:
shadow:x:42:
utmp:x:43:
video:x:44:
sasl:x:45:
plugdev:x:46:
staff:x:50:
games:x:60:
users:x:100:tokyo
nogroup:x:65534:
systemd-journal:x:999:
systemd-network:x:998:
crontab:x:997:
dhcpcd:x:996:
messagebus:x:995:
syslog:x:101:
input:x:994:
sgx:x:993:
clock:x:992:
kvm:x:991:
render:x:990:
systemd-resolve:x:989:
_chrony:x:988:
tss:x:987:
lxd:x:102:ubuntu
uuidd:x:103:
_ssh:x:104:
rdma:x:105:
tcpdump:x:985:
landscape:x:106:
fwupd-refresh:x:984:
polkitd:x:983:
admin:x:107:
netdev:x:108:
ubuntu:x:1000:
tokyo:x:1001:
berlin:x:1002:
professor:x:1003:
developers:x:1004:tokyo,berlin
admins:x:1005:berlin,professor

## Task 4: Shared Directory (20 minutes)
# Create directory: /opt/dev-project
# Set group owner to developers
# Set permissions to 775 (rwxrwxr-x)
# Test by creating files as tokyo and berlin
# Verify: Check permissions and test file creation

ubuntu@ip-172-31-14-118:/$ sudo mkdir /opt/dev-project
ubuntu@ip-172-31-14-118:/$ cd /opt/dev-project/

ubuntu@ip-172-31-14-118:/opt/dev-project$ sudo chown :developers /opt/dev-project
ubuntu@ip-172-31-14-118:/opt/dev-project$ sudo chmod 775 /opt/dev-project
ubuntu@ip-172-31-14-118:/opt/dev-project$ ls -ld /opt/dev-project
drwxrwxr-x 2 root developers 4096 May 25 10:58 /opt/dev-project


ubuntu@ip-172-31-14-118:/opt/dev-project$ sudo su - tokyo
tokyo@ip-172-31-14-118:~$ touch /opt/dev-project/tokyo-file.txt
tokyo@ip-172-31-14-118:~$ exit
logout

ubuntu@ip-172-31-14-118:/$ sudo su - berlin
$ whoami
berlin
$ touch /opt/dev-project/berlin-file.txt
$ exit


ubuntu@ip-172-31-14-118:/$ ls -l /opt/dev-project
total 0
-rw-rw-r-- 1 berlin berlin 0 May 25 11:05 berlin-file.txt
-rw-rw-r-- 1 tokyo  tokyo  0 May 25 11:03 tokyo-file.txt

## Task 5: Team Workspace (20 minutes)
# Create user nairobi with home directory
# Create group project-team
# Add nairobi and tokyo to project-team
# Create /opt/team-workspace directory
# Set group to project-team, permissions to 775
# Test by creating file as nairobi


ubuntu@ip-172-31-14-118:/$ sudo adduser  nairobi 
New password: 
Retype new password:
 passwd: password updated successfully
Changing the user information for nairobi
Enter the new value, or press ENTER for the default
        Full Name []: nairobi
        Room Number []: 
        Work Phone []: 
        Home Phone []: 
        Other []: 
Is the information correct? [Y/n] y
ubuntu@ip-172-31-14-118:/$ sudo groupadd project-team

ubuntu@ip-172-31-14-118:/$ sudo gpasswd -a nairobi project-team
Adding user nairobi to group project-team
ubuntu@ip-172-31-14-118:/$ sudo gpasswd -a tokyo  project-team
Adding user tokyo to group project-team

ubuntu@ip-172-31-14-118:/$ sudo mkdir /opt/team-workspace
ubuntu@ip-172-31-14-118:/$ sudo chown :project-team /opt/team-workspace

ubuntu@ip-172-31-14-118:/$ sudo chmod 775 /opt/team-workspace

ubuntu@ip-172-31-14-118:/$ sudo su nairobi
nairobi@ip-172-31-14-118:/$ touch /opt/team-workspace/nairobi-file.txt
nairobi@ip-172-31-14-118:/$ ls -ld /opt/team-workspace
drwxrwxr-x 2 root project-team 4096 May 25 11:22 /opt/team-workspace