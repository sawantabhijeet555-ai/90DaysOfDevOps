Task 1: Understanding Ownership (10 minutes)
Run ls -l in your home directory

root@ip-172-31-14-118:/home# ls -l
total 20
drwxr-x--- 2 berlin    berlin    4096 May 25 09:56 berlin
drwxr-x--- 2 nairobi   nairobi   4096 May 25 11:18 nairobi
drwxr-x--- 2 professor professor 4096 May 25 09:57 professor
drwxr-x--- 2 tokyo     tokyo     4096 May 28 06:22 tokyo
drwxr-x--- 6 ubuntu    ubuntu    4096 May 27 11:30 ubuntu

Identify the owner and group columns

drwxr-x--- 2 berlin    berlin    4096 May 25 09:56 berlin
             Owner     Group

Owner (u): The single user account that owns the file. By default, this is the person who created it. The owner holds the highest degree of authority over the file and can change its access permissions at any time.

Group (g): A collection of users clustered together for shared access or collaborative projects. If a user belongs to the group assigned to a file, they will inherit whatever permissions are set for this category. A file can only be mapped to one group at a time.

Check who owns your files

-rwxrwxrwx 1 tokyo tokyo 0 May 28 06:22 test.txt

In test.txt - The user , group and other user have read write and execute  permissions. 

----------------------------------------------------------------------------------------------------------------------------------------------

Task 2: Basic chown Operations (20 minutes)
Create file devops-file.txt

ubuntu@ip-172-31-14-118:~/Day-11$ touch devops-file.txt

Check current owner: ls -l devops-file.txt

ubuntu@ip-172-31-14-118:~/Day-11$ ls -l
total 0
-rw-rw-r-- 1 ubuntu ubuntu 0 May 27 11:31 day-11-file-ownership.md
-rw-rw-r-- 1 ubuntu ubuntu 0 May 28 08:48 devops-file.txt

Change owner to tokyo (create user if needed)

ubuntu@ip-172-31-14-118:~$ sudo chown -R tokyo /home/ubuntu/Day-11 
ubuntu@ip-172-31-14-118:~$ ls -l /home/ubuntu/Day-11/devops-file.txt 
-rw-rw-r-- 1 tokyo ubuntu 0 May 28 08:48 /home/ubuntu/Day-11/devops-file.txt

Change owner to berlin

ubuntu@ip-172-31-14-118:~$ sudo chown -R berlin /home/ubuntu/Day-11 
ubuntu@ip-172-31-14-118:~$ ls -l /home/ubuntu/Day-11/devops-file.txt 
-rw-rw-r-- 1 berlin ubuntu 0 May 28 08:48 /home/ubuntu/Day-11/devops-file.txt


-----------------------------------------------------------------------------------------------------------------------------------

Task 3: Basic chgrp Operations (15 minutes)
Create file team-notes.txt

ubuntu@ip-172-31-14-118:~$ pwd
/home/ubuntu
ubuntu@ip-172-31-14-118:~$ touch team-notes.txt
ubuntu@ip-172-31-14-118:~$ ls
Day-11  access-log.txt  day-08-cloud-deployment.md  day-10  private.pem  team-notes.txt

Check current group: ls -l team-notes.txt

ubuntu@ip-172-31-14-118:~$ ls -l team-notes.txt 
-rw-rw-r-- 1 ubuntu ubuntu 0 May 28 09:02 team-notes.txt

Create group: sudo groupadd heist-team

ubuntu@ip-172-31-14-118:~$ sudo groupadd  heist-team

Change file group to heist-team

ubuntu@ip-172-31-14-118:/home$ ls -l /home/ubuntu/Day-11/devops-file.txt 
-rw-rw-r-- 1 berlin ubuntu 0 May 28 08:48 /home/ubuntu/Day-11/devops-file.txt
ubuntu@ip-172-31-14-118:/home$ sudo chgrp heist-team /home/ubuntu/Day-11/devops-file.txt 
ubuntu@ip-172-31-14-118:/home$ ls -l /home/ubuntu/Day-11/devops-file.txt 
-rw-rw-r-- 1 berlin heist-team 0 May 28 08:48 /home/ubuntu/Day-11/devops-file.txt


------------------------------------------------------------------------------------------------------------------------------------------


Task 5: Recursive Ownership (20 minutes)
Create directory structure:

mkdir -p heist-project/vault

mkdir -p heist-project/plans
touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.conf

ubuntu@ip-172-31-14-118:~/heist-project$ ls -l
total 8
drwxrwxr-x 2 ubuntu ubuntu 4096 May 29 05:54 plans
drwxrwxr-x 2 ubuntu ubuntu 4096 May 29 05:56 vault

Create group planners: sudo groupadd planners

ubuntu@ip-172-31-14-118:~/heist-project$ sudo groupadd planner
ubuntu@ip-172-31-14-118:~/heist-project$ cat /etc/group

project-team:x:1007:nairobi,tokyo
heist-team:x:1008:
planners:x:1009:
planner:x:1010:

Change ownership of entire heist-project/ directory:

Owner: professor

ubuntu@ip-172-31-14-118:~$ sudo chown -R professor heist-project
ubuntu@ip-172-31-14-118:~$ ls -l heist-project/
total 8
drwxrwxr-x 2 professor ubuntu 4096 May 29 05:54 plans
drwxrwxr-x 2 professor ubuntu 4096 May 29 05:56 vault

Group: planners

ubuntu@ip-172-31-14-118:~$ sudo chown -R :planner  heist-project
ubuntu@ip-172-31-14-118:~$ ls -l heist-project
total 8
drwxrwxr-x 2 professor planner 4096 May 29 05:54 plans
drwxrwxr-x 2 professor planner 4096 May 29 05:56 vault

Use recursive flag (-R)
Verify all files and subdirectories changed: ls -lR heist-project/

----------------------------------------------------------------------------------------------------------------------------------------




Create users: tokyo, berlin, nairobi (if not already created)

Create groups: vault-team, tech-team

ubuntu@ip-172-31-14-118:~$ sudo groupadd vault-team
ubuntu@ip-172-31-14-118:~$ sudo groupadd tech-team
ubuntu@ip-172-31-14-118:~$ cat /etc/group

Create directory: bank-heist/

ubuntu@ip-172-31-14-118:~$ mkdir bank-heist
ubuntu@ip-172-31-14-118:~$ ls
Day-11  access-log.txt  bank-heist  day-08-cloud-deployment.md  day-10  heist-project  private.pem  team-notes.txt

Create 3 files inside:

touch bank-heist/access-codes.txt
touch bank-heist/blueprints.pdf
touch bank-heist/escape-plan.txt

ubuntu@ip-172-31-14-118:~$ cd bank-heist/
ubuntu@ip-172-31-14-118:~/bank-heist$ touch access-codes.txt
ubuntu@ip-172-31-14-118:~/bank-heist$ ls
access-codes.txt
ubuntu@ip-172-31-14-118:~/bank-heist$ touch blueprints.pdf
ubuntu@ip-172-31-14-118:~/bank-heist$ ls
access-codes.txt  blueprints.pdf
ubuntu@ip-172-31-14-118:~/bank-heist$ touch escape-plan.txt
ubuntu@ip-172-31-14-118:~/bank-heist$ ls 
access-codes.txt  blueprints.pdf  escape-plan.txt


Set different ownership:

access-codes.txt → owner: tokyo, group: vault-team

ubuntu@ip-172-31-14-118:~/bank-heist$ sudo chown -R tokyo access-codes.txt 
ubuntu@ip-172-31-14-118:~/bank-heist$ su tokyo
Password: 
tokyo@ip-172-31-14-118:/home/ubuntu/bank-heist$ ls
access-codes.txt  blueprints.pdf  escape-plan.txt
tokyo@ip-172-31-14-118:/home/ubuntu/bank-heist$ ls -l access-codes.txt 
-rw-rw-r-- 1 tokyo ubuntu 0 May 29 06:10 access-codes.txt

ubuntu@ip-172-31-14-118:~/bank-heist$ sudo chown -R :vault-team  access-codes.txt 
ubuntu@ip-172-31-14-118:~/bank-heist$ la -l access-codes.txt 
-rw-rw-r-- 1 tokyo vault-team 0 May 29 06:10 access-codes.txt

blueprints.pdf → owner: berlin, group: tech-team
escape-plan.txt → owner: nairobi, group: vault-team


