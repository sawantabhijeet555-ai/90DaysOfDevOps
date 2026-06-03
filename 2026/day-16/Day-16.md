DAy-16 

## Task 1: Your First Script
# Create a file hello.sh

ubuntu@ip-172-31-14-118:~/Day-16$ touch hello.sh

ubuntu@ip-172-31-14-118:~/Day-16$ ls -l
total 4
-rw-rw-r-- 1 ubuntu ubuntu 36 Jun  3 04:51 hello.sh

## Add the shebang line #!/bin/bash at the top
# Print Hello, DevOps! using echo

ubuntu@ip-172-31-14-118:~/Day-16$ vim hello.sh 

#!/bin/bash

echo "Hello, DevOps !"

##Make it executable and run it
# chmod +x hello.sh
# ./hello.sh

ubuntu@ip-172-31-14-118:~/Day-16$ chmod +x hello.sh
ubuntu@ip-172-31-14-118:~/Day-16$ ls -l
total 4
-rwxrwxr-x 1 ubuntu ubuntu 36 Jun  3 04:51 hello.sh


ubuntu@ip-172-31-14-118:~/Day-16$ ./hello.sh
Hello, DevOps!

----------------------------------------------------------------------------------------------------------------------------------------------

## Task 2: Variables
# Create variables.sh with:

ubuntu@ip-172-31-14-118:~/Day-16$ touch variable.sh
ubuntu@ip-172-31-14-118:~/Day-16$ vim variable.sh 
ubuntu@ip-172-31-14-118:~/Day-16$ chmod +x variable.sh 

# A variable for your NAME
# A variable for your ROLE (e.g., "DevOps Engineer")
# Print: Hello, I am <NAME> and I am a <ROLE>

ubuntu@ip-172-31-14-118:~/Day-16$ vim variable.sh 

#!/bin/bash

NAME="Abhijeet"
ROLE="DevOps Engineer"

echo "Hello , I am $NAME , I am $ROLE"

ubuntu@ip-172-31-14-118:~/Day-16$ ./variable.sh 
Hello , I am Abhijeet , I am DevOps Engineer 


------------------------------------------------------------------------------------------------------------------------------------------------

## Task 3: User Input with read
# Create greet.sh that:


ubuntu@ip-172-31-14-118:~/Day-16$ touch greet.sh
ubuntu@ip-172-31-14-118:~/Day-16$ vim greet.sh 
ubuntu@ip-172-31-14-118:~/Day-16$ chmod +x greet.sh

# Asks the user for their name using read
# Asks for their favourite tool


ubuntu@ip-172-31-14-118:~/Day-16$ cat greet.sh 
#!/bin/bash 

read -p "What is you Name" NAME
read -p "What is your Favourite Tool" TOOL
 
echo "Hello $NAME , Your favourite tool is $TOOL " 

# Prints: Hello <name>, your favourite tool is <tool>

ubuntu@ip-172-31-14-118:~/Day-16$ ./greet.sh 
What is you Nameabhijeet
What is your Favourite Tooldocker
Hello abhijeet , Your favourite tool is docker

------------------------------------------------------------------------------------------------------------------------------------------------

## Task 4: If-Else Conditions
# Create check_number.sh that:

# Takes a number using read
# Prints whether it is positive, negative, or zero


#!/bin/bash

read -p "Enter number " Number 

if (($Number > 0))
then
        echo "Number is positive"
elif (($Number < 0  ))
then
        echo "Number is Negative "
else    echo "Number is Zero "

fi

Output : 

ubuntu@ip-172-31-14-118:~/Day-16$ ./check_number.sh 
Enter number 5
Number is positive
ubuntu@ip-172-31-14-118:~/Day-16$ ./check_number.sh 
Enter number -7
Number is Negative 
ubuntu@ip-172-31-14-118:~/Day-16$ ./check_number.sh 
Enter number 0
Number is Zero 

------------------------------------------------------------------------------------------------------------------------------------------------
# Create file_check.sh that:

# Asks for a filename
# Checks if the file exists using -f
# Prints appropriate message


ubuntu@ip-172-31-14-118:~/Day-16$ cat file_check.sh 
#!/bin/bash

read -p "Please enter file name : " File 

if [ -f $File ]
then
        echo "The File $File is exites. "
else 
        echo "The File $File is not exites. "
fi

------------------------------------------------------------------------------------------------------------------------------------------------
## Task 5: Combine It All
# Create server_check.sh that:

# Stores a service name in a variable (e.g., nginx, sshd)
# Asks the user: "Do you want to check the status? (y/n)"
# If y — runs systemctl status <service> and prints whether it's active or not
# If n — prints "Skipped."

ubuntu@ip-172-31-14-118:~/Day-16$ cat server_check.sh 
#!/bin/bash

SERVICE="nginx"

read -p "Do you want to check the status of $SERVICE? (y/n): " CHOICE

case "$CHOICE" in
    [Yy]* )
        echo "Checking $SERVICE status..."
        
        if systemctl is-active --quiet "$SERVICE"; then
            echo "Result: $SERVICE is ACTIVE."
        else
            echo "Result: $SERVICE is INACTIVE (or not found)."
        fi
        
        echo "--------------------------------"
        systemctl status "$SERVICE" --no-pager
        ;;
        
    [Nn]* )
        echo "Skipped."
        ;;
        
    * )
        echo "Invalid input. Operation aborted."
        exit 1
        ;;
esac




ubuntu@ip-172-31-14-118:~/Day-16$ ./server_check.sh 
Do you want to check the status of nginx? (y/n): y
Checking nginx status...
Result: nginx is ACTIVE.
--------------------------------
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-06-03 04:45:33 UTC; 4h 34min ago
 Invocation: 2ac0fe751aed4b04be628d94be6bf1dd
       Docs: man:nginx(8)
    Process: 701 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 731 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 761 (nginx)
      Tasks: 2 (limit: 657)
     Memory: 4M (peak: 4.3M)
        CPU: 44ms
     CGroup: /system.slice/nginx.service
             ├─761 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             └─763 "nginx: worker process"

Jun 03 04:45:32 ip-172-31-14-118 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
Jun 03 04:45:33 ip-172-31-14-118 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.


ubuntu@ip-172-31-14-118:~/Day-16$ ./server_check.sh 
Do you want to check the status of nginx? (y/n): n
Skipped.