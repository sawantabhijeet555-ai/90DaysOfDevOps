Processes Command 

1) ps    - display information about currently running processes.

        I) ps -e : Display All Running Processes -
             The -e option displays all running processes on the system. Use this option when you need a complete list of system processes,
             including background services and processes from other users.


        II)ps -f : 
             Display Full Process Format - The -f option displays full-format listing, including additional columns like parent process ID.  Use
             this option when you want detailed process information, such as process hierarchy and user ID.
        
        III)ps -f : Display Full Process Format
             The -f option displays full-format listing, including additional columns like parent process ID. Use this option when you want
             detailed process information, such as process hierarchy and user ID.
     
        IV) ps -u UserName: Display Processes for a Specific User  Input :- ps -u root
             The -u option filters processes based on the user who started them. This option is useful when monitoring the activity of a
             particular user on a multi-user system.

         v) ps -x : Show Processes Without a Controlling Terminal
             The -x option displays processes that do not have a controlling terminal (TTY). These processes usually run in the background and
             include system services and daemons.
  
         VI)ps -a : Display Processes for All Users With a Terminal
             The -a option lists processes for all users that are associated with a terminal, except session leaders.

         VII)ps -p PID : Display Process by Process ID
             Every process in Linux has a unique Process ID (PID). The -p option allows users to display information about specific processes
             using their Process IDs (PIDs). Multiple PIDs can be provided as a space-separated or comma-separated list.
------------------------------------------------------------------------------------------------------------------------------------------------
2) top   - Interactive and real-time process monitoring tool in Linux. It continuously updates system information such as CPU usage, memory
           usage, and running processes. Unlike ps, which provides a static snapshot, top updates the process list automatically every few
           seconds.

        -I)htop   - Provide colourful Interactive UI
------------------------------------------------------------------------------------------------------------------------------------------------
3) kill PID - It is commonly used to terminate processes, but it can also pause, resume, or perform other actions depending on the signal sent.
              The kill command sends signals to processes using their Process ID (PID).
         
        -I) kill -9 PID -Forcefully kill process.
        -II)kill -15 PID - Safe termination.
       -III)kill -1 PID - Restart /Read process.
------------------------------------------------------------------------------------------------------------------------------------------------
Network Command 

1) Ping     - Checks internet or server connectivity
            Syntax :- ping google.com

------------------------------------------------------------------------------------------------------------------------------------------------
2) ifconfig - command for network interface details 
            Install if missing 
            - sudo apt-get install net-tools -y
------------------------------------------------------------------------------------------------------------------------------------------------3)ss -tulnp  - Shows open ports and connections. (Modern replacement of netstat)
  netstat -tulnp
------------------------------------------------------------------------------------------------------------------------------------------------
4) nslookup - DNS lookup command
    e.g. ->nslookup google.com
    scenario :- Verify DNS resolution
------------------------------------------------------------------------------------------------------------------------------------------------
5)dig - Advanced DNS query Tool
       Install if missing
       sudo apt-get install dnsutails -y
    e.g. dig google.com
------------------------------------------------------------------------------------------------------------------------------------------------
6)curl -Test APIs or websites.
    e.g. :-curl http://googl.com
    scenario :- Check website response
------------------------------------------------------------------------------------------------------------------------------------------------
 
7) traceroute - Shows route packets travel.
     e.g.:- traceroute google.com 
     scenario :- Identify network delay.

------------------------------------------------------------------------------------------------------------------------------------------------
File system Commands

1) cp filename1 filename2  - Copy File
   eg :-cp hello.txt backup.txt
------------------------------------------------------------------------------------------------------------------------------------------------ 
2) cp -r Folder1/  Folder2/ - Copy Folder
   e.g:- cp -r DevOps/ backup/
------------------------------------------------------------------------------------------------------------------------------------------------ 
3) mv filename1 filename2  - Move or Rename File
   e.g:-mv backup.txt new.txt
------------------------------------------------------------------------------------------------------------------------------------------------
4) mv filename Location    -Move file on specific Location
   e.g. :- mv new.txt /home/ubuntu/backup
------------------------------------------------------------------------------------------------------------------------------------------------
5) find /home -name <filename> - Search File
   e.g. :- find /home -name hello.txt
------------------------------------------------------------------------------------------------------------------------------------------------
6) locate Filename - Quickly Find File
   Install if missing
   sudo apt-get install plocate
  e.g:- locate hello.txt
------------------------------------------------------------------------------------------------------------------------------------------------
7) du -sh Location - Check Folder Size 
  e.g. du -sh /home/ubuntu/DevOps
------------------------------------------------------------------------------------------------------------------------------------------------
8) grep "txt" filename - Search Text in file
e.g. grep "dosto" hello.txt
Finds lines Contaning dosto
------------------------------------------------------------------------------------------------------------------------------------------------
9)chmod 755 filename
  permission Meaning :
  - 7-->Read + Write + Execute
  - 5--> Read + Execute
  e.g.:- chmod 755 hello.txt
------------------------------------------------------------------------------------------------------------------------------------------------
10)chown - Change Owwnership
 chown ubuntu:ubuntu hello.txt