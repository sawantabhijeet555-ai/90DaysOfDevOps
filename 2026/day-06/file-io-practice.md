##### File I/O Practice

## Create File 
ubuntu@ip-172-31-13-255:~$ touch notes.txt

# Check file created or not 
ubuntu@ip-172-31-13-255:~$ ls

# Expected Output 
DevOps  Linux-practice.md  amazon-cloudwatch-agent.deb  backup  file-io-practice.md  notes.txt

--------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Write Frist Line 
ubuntu@ip-172-31-13-255:~$ echo "Linux File oractice started. " > notes.txt

# What this does 
  # Echo print Text
  # > Writess text into file 
  # Old Content gets Replaced 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Append Sceond Line 

ubuntu@ip-172-31-13-255:~$ echo "learning file handling commands. " >> notes.txt

# What this does 
  # >> Appends content
  # Existing content stays safe
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Append Thrid Line Using tee

ubuntu@ip-172-31-13-255:~$ echo "tee command writes and display outout. " | tee -a notes.txt

# What this does 
  # tee Writes into file
  # Also display output on terminal 
  # -a means append mode

# expected terminal Output
 
tee command writes and display outout. 
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Read Full File 
 
ubuntu@ip-172-31-13-255:~$ cat notes.txt

# Expected Output 

Linux File oractice started. 
learning file handling commands. 
tee command writes and display outout. 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Read First Two Lines 
ubuntu@ip-172-31-13-255:~$ head -n 2 notes.txt

# Expected output
Linux File oractice started. 
learning file handling commands. 
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Read Last Two Lines 

ubuntu@ip-172-31-13-255:~$ tail -n 2 notes.txt

# Expected Output 
learning file handling commands. 
tee command writes and display outout.