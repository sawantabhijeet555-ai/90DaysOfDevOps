# Day 39 - CI/CD Concepts

## Task 1: The Problem

### 1. What can go wrong if 5 developers manually deploy to the same production server?

Many issues can occur:

-   Developers overwrite each other's changes.
-   Different developers deploy different versions.
-   Human errors while copying files or running commands.
-   Someone forgets an important deployment step.
-   Production downtime due to incorrect deployment.
-   No deployment history or rollback process.
-   Difficult to identify who deployed what.
-   Configuration differences between environments.
-   Bugs reach production because testing was skipped.

### 2. What does "It works on my machine" mean?

"It works on my machine" means the application runs correctly on the
developer's computer but fails on another developer's system, testing
server, or production.

#### Why is it a real problem?

Because environments are different:

-   Different operating systems
-   Different software versions
-   Missing dependencies
-   Different environment variables
-   Different database configurations

This leads to wasted debugging time and inconsistent deployments.

### 3. How many times a day can a team safely deploy manually?

Usually only **1--3 deployments per day**.

Manual deployments are: - Slow - Error-prone - Difficult to repeat -
Risky during business hours

With CI/CD, teams can safely deploy **dozens or even hundreds of times
per day** because the process is automated and tested.

------------------------------------------------------------------------

## Task 2: CI vs CD

### Continuous Integration (CI)

Continuous Integration is the practice of automatically building and
testing code whenever developers push changes to the shared repository.

It helps detect: - Build failures - Test failures - Integration issues -
Code quality problems

**Example:** A developer pushes code to GitHub. GitHub Actions installs
dependencies, builds the application, and runs unit tests. If tests
fail, the code is not merged.

### Continuous Delivery (CD)

Continuous Delivery extends CI by automatically preparing the
application for deployment.

The application is always in a deployable state, but deployment to
production usually requires **manual approval**.

**Example:** After CI succeeds, a Docker image is built, pushed to
Docker Hub, and deployed to staging. A release manager approves
production deployment.

### Continuous Deployment

Continuous Deployment automatically deploys every successful change to
production without manual approval.

**Example:** A developer pushes code, the pipeline builds, tests,
creates a Docker image, and deploys directly to production.

### Difference

  ------------------------------------------------------------------------
  Continuous Integration    Continuous Delivery    Continuous Deployment
  ------------------------- ---------------------- -----------------------
  Build & Test              Ready for deployment   Automatically deploy

  Automatic                 Automatic              Automatic

  Stops after testing       Manual production      No manual approval
                            approval               
  ------------------------------------------------------------------------

------------------------------------------------------------------------

## Task 3: Pipeline Anatomy

### Trigger

Starts the pipeline.

Examples: - Git push - Pull request - Scheduled job - Manual trigger

### Stage

A logical phase of the pipeline.

Examples: - Build - Test - Deploy

### Job

A collection of related tasks inside a stage.

Example: - Build Backend - Build Frontend

### Step

A single command executed inside a job.

``` bash
npm install
npm test
docker build .
```

### Runner

The machine that executes pipeline jobs.

Examples: - GitHub-hosted runner - Self-hosted runner - Jenkins agent

### Artifact

A file generated during the pipeline that can be used later.

Examples: - Docker image - JAR file - ZIP package - Test report -
Coverage report

------------------------------------------------------------------------

## Task 4: CI/CD Pipeline Diagram


                Developer

                    │
                    ▼
           Push Code to GitHub
                    │
                    ▼
               Trigger
                    │
                    ▼
              Stage 1: Build
                    │
            Install Dependencies
                    │
             Build Application
                    │
                    ▼
              Stage 2: Test
                    │
             Run Unit Tests
                    │
       Run Integration Tests
                    │
                    ▼
          Stage 3: Docker Build
                    │
          Build Docker Image
                    │
          Push Docker Image
                    │
                    ▼
       Stage 4: Deploy to Staging
                    │
           Update Container
                    │
                    ▼
             Staging Server
```

------------------------------------------------------------------------

## Task 5: Explore in the Wild

### Repository

Workflow: `.github/workflows/test.yml`

### What triggers it?

-   Push
-   Pull Request

### How many jobs does it have?

Multiple jobs (around 4--6 depending on the current version),
including: - Lint - Test - Documentation checks - Build validation

### What does it do?

-   Installs Python
-   Installs project dependencies
-   Runs linting
-   Executes unit tests
-   Checks documentation
-   Ensures code quality before merging
