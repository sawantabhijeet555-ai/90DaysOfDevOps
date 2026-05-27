## Task 1: Create Files 
# Create empty file devops.txt using touch

ubuntu@ip-172-31-14-118:~$ sudo mkdir day-10
ubuntu@ip-172-31-14-118:~$ cd day-10/

ubuntu@ip-172-31-14-118:~/day-10$ sudo touch devops.txt
ubuntu@ip-172-31-14-118:~/day-10$ ls
devops.txt

# Create notes.txt with some content using cat or echo

ubuntu@ip-172-31-14-118:~/day-10$ sudo echo "hello dosto ..... Welcome to the zero to hero DevOps class" > notes.txt
ubuntu@ip-172-31-14-118:~/day-10$ cat notes.txt
hello dosto ..... Wlcome to the zero to hero DevOps class


# Create script.sh using vim with content: echo "Hello DevOps"

ubuntu@ip-172-31-14-118:~/day-10$ vim script.sh
ubuntu@ip-172-31-14-118:~/day-10$ echo "hello DevOps" > script.sh 
ubuntu@ip-172-31-14-118:~/day-10$ cat script.sh
hello DevOps

# Verify: ls -l to see permissions

ubuntu@ip-172-31-14-118:~/day-10$ ls -l
total 12
-rw-rw-r-- 1 ubuntu ubuntu 908 May 27 07:18 day-10-file-permissions.md
-rw-r--r-- 1 ubuntu ubuntu   0 May 27 06:38 devops.txt
-rwxrwxr-- 1 ubuntu ubuntu  58 May 27 06:48 notes.txt
-rw-rw-r-- 1 ubuntu ubuntu  13 May 27 07:15 script.sh

-------------------------------------------------------------------------------------------------------------------------------
## Task 2: Read Files (10 minutes)

# Read notes.txt using cat

ubuntu@ip-172-31-14-118:~/day-10$ echo "Wlcome to the DevOps " > notes.txt
ubuntu@ip-172-31-14-118:~/day-10$ cat notes.txt
Welcome to the DevOps 

# View script.sh in vim read-only mode

ubuntu@ip-172-31-14-118:~/day-10$ chmod 400 script.sh
ubuntu@ip-172-31-14-118:~/day-10$ ls -l
total 12
-rw-rw-r-- 1 ubuntu ubuntu 1229 May 27 07:20 day-10-file-permissions.md
-rw-r--r-- 1 ubuntu ubuntu    0 May 27 06:38 devops.txt
-rwxrwxr-- 1 ubuntu ubuntu   22 May 27 07:20 notes.txt
-r-------- 1 ubuntu ubuntu   13 May 27 07:15 script.sh

# Display first 5 lines of /etc/passwd using head

ubuntu@ip-172-31-14-118:~/day-10$ sudo head -n 5 /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync

# Display last 5 lines of /etc/passwd using tail

ubuntu@ip-172-31-14-118:~/day-10$ sudo tail -n 5 /etc/passwd
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
tokyo:x:1001:1001:Abhijeet,100,+91 8275628402,+91 8788493697:/home/tokyo:/bin/bash
berlin:x:1002:1002::/home/berlin:/bin/sh
professor:x:1003:1003::/home/professor:/bin/sh
nairobi:x:1006:1006:nairobi,,,:/home/nairobi:/bin/bash

-----------------------------------------------------------------------------------------------------------------

## Task 3: Understand Permissions (10 minutes)
# Format: rwxrwxrwx (owner-group-others)

r = read (4), w = write (2), x = execute (1)
Check your files: ls -l devops.txt notes.txt script.sh

Answer: What are current permissions? Who can read/write/execute?

ubuntu@ip-172-31-14-118:~/day-10$ ls -l
total 12
-rw-rw-r-- 1 ubuntu ubuntu 2510 May 27 09:24 day-10-file-permissions.md
-rw-r--r-- 1 ubuntu ubuntu    0 May 27 06:38 devops.txt
-rwxrwxr-- 1 ubuntu ubuntu   22 May 27 07:20 notes.txt
-r-------- 1 ubuntu ubuntu   13 May 27 07:15 script.sh


In devops.txt - The group and other user  have read-only permissions  and The users have read and write permissions .
In notes.txt - The user and group have read , write and Execute permissions, and other users have read-only permissions .
script.sh - Only user have read-only permission.

--------------------------------------------------------------------------------------------------------------------
## Task 4: Modify Permissions (20 minutes)
# Make script.sh executable → run it with ./script.sh

ubuntu@ip-172-31-14-118:~/day-10$ chmod +x script.sh

ubuntu@ip-172-31-14-118:~/day-10$ ls -l
total 12
-rw-rw-r-- 1 ubuntu ubuntu 3580 May 27 10:52 day-10-file-permissions.md
-rw-r--r-- 1 ubuntu ubuntu    0 May 27 06:38 devops.txt
-rwxrwxr-- 1 ubuntu ubuntu   22 May 27 07:20 notes.txt
-r-x--x--x 1 ubuntu ubuntu   13 May 27 10:55 script.sh


# Set devops.txt to read-only (remove write for all)

ubuntu@ip-172-31-14-118:~/day-10$ chmod 444 devops.txt 
ubuntu@ip-172-31-14-118:~/day-10$ ls -l devops.txt 
-r--r--r-- 1 ubuntu ubuntu 0 May 27 06:38 devops.txt

# Set notes.txt to 640 (owner: rw, group: r, others: none)

ubuntu@ip-172-31-14-118:~/day-10$ chmod 640 notes.txt
ubuntu@ip-172-31-14-118:~/day-10$ ls -l notes.txt 
-rw-r----- 1 ubuntu ubuntu 22 May 27 07:20 notes.txt

# Create directory project/ with permissions 755

ubuntu@ip-172-31-14-118:~/day-10$ mkdir project

ubuntu@ip-172-31-14-118:~/day-10$ ls -l
total 16
-rw-rw-r-- 1 ubuntu ubuntu 3580 May 27 10:52 day-10-file-permissions.md
-r--r--r-- 1 ubuntu ubuntu    0 May 27 06:38 devops.txt
-rw-r----- 1 ubuntu ubuntu   22 May 27 07:20 notes.txt
drwxr-xr-x 2 ubuntu ubuntu 4096 May 27 11:05 project
-r-x--x--x 1 ubuntu ubuntu   49 May 27 11:01 script.sh

---------------------------------------------------------------------------------------------------------------------------------------------
## Task 5: Test Permissions (10 minutes)
# Try writing to a read-only file - what happens?

ubuntu@ip-172-31-14-118:~/day-10$ ls -l test.txt
-r--r--r-- 1 ubuntu ubuntu 0 May 27 11:13 test.txt
ubuntu@ip-172-31-14-118:~/day-10$ echo "hello dosto " > test.txt
-bash: test.txt: Permission denied

# Try executing a file without execute permission

ubuntu@ip-172-31-14-118:~/day-10$ ./test.sh
-bash: ./test.sh: Permission denied
ubuntu@ip-172-31-14-118:~/day-10$ ls -l test.sh
-rw-rw-rw- 1 ubuntu ubuntu 55 May 27 11:16 test.sh

----------------------------------------------------------------------------------------------------------------------------------------------

