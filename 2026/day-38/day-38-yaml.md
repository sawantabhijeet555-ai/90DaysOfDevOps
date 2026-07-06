## Task 1: Key-Value Pairs
<!-- Create person.yaml that describes yourself with:

name
role
experience_years
learning (a boolean)
Verify: Run cat person.yaml — does it look clean? No tabs?
 -->

ubuntu@ip-172-31-14-118:~/Day-38$ cat person.yml 
name: abhijeet
role: cloud engineer 
experience_year: 2
learning: true
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Lists
<!-- Add to person.yaml:

tools — a list of 5 DevOps tools you know or are learning
hobbies — a list using the inline format [item1, item2] -->

ubuntu@ip-172-31-14-118:~/Day-38$ cat person.yml 
name: Abhijeet
role: Cloud Engineer 
experience_year: 2
learning: true

tools:
  - Linux
  - Docker
  - Jenkins
  - K8s
  - Git

hobbies: ['acting','dancing','playing cricket','watching movies']

Write in your notes: What are the two ways to write a list in YAML?

Block style → Each item is on a new line with -.
Inline (Flow) style → All items are written on a single line inside []

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Nested Objects
<!-- Create server.yaml that describes a server:

server with nested keys: name, ip, port
database with nested keys: host, name, credentials (nested further: user, password)
Verify: Try adding a tab instead of spaces — what happens when you validate it? -->

ubuntu@ip-172-31-14-118:~/Day-38$ cat server.yml 
server:
  name: ubuntu 22.04
  ip: 12.255.255.255
  port: 80

database:
  host: postgres
  name: test_db
  credentials:
    user: root
    password: test@123
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 4: Multi-line Strings
<!-- In server.yaml, add a startup_script field using:

The | block style (preserves newlines)
The > fold style (folds into one line) -->

server:
  name: ubuntu 22.04
  ip: 12.255.255.255
  port: 80

  startup_script_pipe: |
    #!/bin/bash
    sudo apt update
    sudo apt install -y nginx
    sudo systemctl start nginx
    sudo systemctl enable nginx

  startup_script_fold: >
    #!/bin/bash
    sudo apt update
    sudo apt install -y nginx
    sudo systemctl start nginx
    sudo systemctl enable nginx

database:
  host: postgres
  name: test_db
  credentials:
    user: root
    password: test@123

Write in your notes: When would you use | vs >?

| (Literal Block Style): Preserves line breaks exactly as written. Use it for scripts, configuration files, certificates, or any content where newlines matter.

> (Folded Block Style): Folds multiple lines into a single line by replacing newlines with spaces. Use it for long descriptions, messages, or paragraphs.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 5: Validate Your YAML
<!-- Install yamllint or use an online validator
Validate both your YAML files
Intentionally break the indentation — what error do you get?
Fix it and validate again -->


ubuntu@ip-172-31-14-118:~/Day-38$ sudo apt-get install yamllint -y

ubuntu@ip-172-31-14-118:~/Day-38$ yamllint server.yml 
server.yml
  1:1       warning  missing document start "---"  (document-start)

ubuntu@ip-172-31-14-118:~/Day-38$ cat server.yml 
server:
  name: ubuntu 22.04
  ip: 12.255.255.255
  port: 80

database:
  host: postgres
  name: test_db
  credentials:
    user: root
    password: test@123
ubuntu@ip-172-31-14-118:~/Day-38$ yamllint person.yml 
person.yml
  1:1       warning  missing document start "---"  (document-start)
  2:21      error    trailing spaces  (trailing-spaces)
  13:20     error    too few spaces after comma  (commas)
  13:30     error    too few spaces after comma  (commas)
  13:48     error    too few spaces after comma  (commas)

ubuntu@ip-172-31-14-118:~/Day-38$ cat person.yml 
name: Abhijeet
role: Cloud Engineer 
experience_year: 2
learning: true

tools:
  - Linux
  - Docker
  - Jenkins
  - K8s
  - Git

hobbies: ['acting','dancing','playing cricket','watching movies']
ubuntu@ip-172-31-14-118:~/Day-38$ vim person.yml 
ubuntu@ip-172-31-14-118:~/Day-38$ yamllint person.yml 
person.yml
  1:1       warning  missing document start "---"  (document-start)
  13:20     error    too few spaces after comma  (commas)
  13:30     error    too few spaces after comma  (commas)
  13:48     error    too few spaces after comma  (commas)

ubuntu@ip-172-31-14-118:~/Day-38$ vim person.yml 
ubuntu@ip-172-31-14-118:~/Day-38$ yamllint person.yml 
person.yml
  1:1       warning  missing document start "---"  (document-start)
  13:20     error    too few spaces after comma  (commas)
  13:30     error    too few spaces after comma  (commas)
  13:48     error    too few spaces after comma  (commas)

ubuntu@ip-172-31-14-118:~/Day-38$ vim person.yml 
ubuntu@ip-172-31-14-118:~/Day-38$ yamllint person.yml 
person.yml
  1:1       warning  missing document start "---"  (document-start)
  13:20     error    too few spaces after comma  (commas)
  13:30     error    too few spaces after comma  (commas)
  13:48     error    too few spaces after comma  (commas)

ubuntu@ip-172-31-14-118:~/Day-38$ cat person.yml 
name: Abhijeet
role: Cloud_Engineer
experience_year: 2
learning: true

tools:
  - Linux
  - Docker
  - Jenkins
  - K8s
  - Git

hobbies: ['acting','dancing','playing cricket','watching movies']
ubuntu@ip-172-31-14-118:~/Day-38$ vim person.yml 
ubuntu@ip-172-31-14-118:~/Day-38$ yamllint person.yml 
person.yml
  1:1       warning  missing document start "---"  (document-start)


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Task 6: Spot the Difference
<!-- Read both blocks and write what's wrong with the second one:

# Block 1 - correct
name: devops
tools:
  - docker
  - kubernetes
# Block 2 - broken
name: devops
tools:
- docker
  - kubernetes -->

The second block has inconsistent indentation. The first list item (- docker) is not indented under tools, while the second item is. In YAML, all items in a list must be indented consistently under their parent key.


