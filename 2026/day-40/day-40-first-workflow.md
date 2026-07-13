Task 1: Set Up
Create a new public GitHub repository called github-actions-practice
Clone it locally
Create the folder structure: .github/workflows/

Abhijeet Sawant@IMSMUMDTO01182 MINGW64 ~/Documents/Github-for-devops/github-action-parctice/github-action-parctice (main)
$ ls -a
./  ../  .git/  .github/  app.py  github-action-parctice/  README.md

Abhijeet Sawant@IMSMUMDTO01182 MINGW64 ~/Documents/Github-for-devops/github-action-parctice/github-action-parctice (main)
$ cd .github/

Abhijeet Sawant@IMSMUMDTO01182 MINGW64 ~/Documents/Github-for-devops/github-action-parctice/github-action-parctice/.github (main)
$ ls
workflows/

Abhijeet Sawant@IMSMUMDTO01182 MINGW64 ~/Documents/Github-for-devops/github-action-parctice/github-action-parctice/.github (main)
$ cd workflows/

Task 2: Hello Workflow
Create .github/workflows/hello.yml with a workflow that:

Triggers on every push
Has one job called greet
Runs on ubuntu-latest
Has two steps:
Step 1: Check out the code using actions/checkout
Step 2: Print Hello from GitHub Actions!
Push it. Go to the Actions tab on GitHub and watch it run.

Verify: Is it green? Click into the job and read every step.

name: CI Build Update
on:
  push:
    branches: ["main"]

jobs:
  Demo_Hello:
      runs-on: ubuntu-latest

      steps:
        - name: code checkout
          uses: actions/checkout@v4

        - name:  Print Hello from GitHub Actions!
          run: echo "Hello from GitHub Actions!"


Task 3: Understand the Anatomy
Look at your workflow file and write in your notes what each key does:

on:
jobs:
runs-on:
steps:
uses:
run:
name: (on a step)

name: The title of the whole project.

on: The Trigger.

	Example: When someone pushes open the doors to the "kitchen", start the process.

jobs: The Container grouping the workers together.

	Example: Defining the "baking crew" department.

runs-on: The Environment where the work happens.

	Example: Giving the crew a clean Linux workstation (ubuntu-latest) to stand at.

steps: The sequential Checklist.

	Example: The ordered recipe steps the crew must follow from top to bottom.

name: (on a step): The Label so you know what's happening.

	Example: "Fetch the Secret Recipe Book" shows up in your GitHub logs.

uses: Borrowing a Pre-made Plugin.

	Example: Instead of writing code to find your files, you "use" GitHub's built-in tool (actions/checkout) to instantly grab your code and 	put it on the kitchen counter.

run: Executing Raw Commands directly in the terminal.

	Example: No pre-made tool exists for your specific custom script, so you tell the runner to manually type and execute terminal commands 	(echo "Mixing...") yourself.


Task 4: Add More Steps
Update hello.yml to also:

Print the current date and time
Print the name of the branch that triggered the run (hint: GitHub provides this as a variable)
List the files in the repo
Print the runner's operating system
Push again — watch the new run.

name: CI Build Update
on:
  push:
    branches: ["main"]

jobs:
  Demo_Hello:
      runs-on: ubuntu-latest

      steps:
        - name: code checkout
          uses: actions/checkout@v4

        - name:  Print Hello from GitHub Actions!
          run: echo "Hello from GitHub Actions!"

        - name: Print the current date and time
          run: |
              echo "Current Date & Time is : $(date '+%Y-%m-%d %H:%M:%S')"

        - name: Print Triggering Branch Name
          run: echo "The branch that triggered this run is ${{ github.ref_name }}"

        - name: List files in the repo
          run: ls -la

        - name: Print the runner's operating system
          run: echo "The operating system is ${{ runner.os }}"



Task 5: Break It On Purpose
Add a step that runs a command that will fail (e.g., exit 1 or a misspelled command)
Fix it and push again

name: CI Build Update
on:
  push:
    branches: ["main"]

jobs:
  Demo_Hello:
      runs-on: ubuntu-latest

      steps:
        - name: code checkout
          uses: actions/checkout@v4

        - name:  Print Hello from GitHub Actions!
          run: echo "Hello from GitHub Actions!"

        - name: Print the current date and time
          run: |
              echo "Current Date & Time is : $(date '+%Y-%m-%d %H:%M:%S')"

        - name: Print Triggering Branch Name
          run: echo "The branch that triggered this run is ${{ github.ref_name }}"

        - name: List files in the repo
          run: ls -la

        - name: Print the runner's operating system
          run: echo "The operating system is ${{ runner.os }}"

        - name: Intentionally Failing Step
          run: fakecommand

Push and observe what happens in the Actions tab

You will see a Red "X" icon next to the run.Click on the run, and you will see that the execution stopped exactly at Intentionally Failing Step.
Notice: If you had steps below the failure, GitHub would automatically skip them because the chain broke.
