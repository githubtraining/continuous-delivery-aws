# Welcome to the course!

We will be working with Continuous Delivery. We will...
- Create a workflow to deploy to staging based on a label
- Create a workflow to deploy to production based on merging to master
- Use AWS configuration

Before you start, you should...
- [Introduction to GitHub](https://lab.github.com/githubtraining/introduction-to-github)
- [Continuous Integration with GitHub Actions](https://lab.github.com/githubtraining/set-up-continuous-integration-with-github-actions)

### What is Continuous Delivery?

According to [continuousdelivery.com](https://continuousdelivery.com/),

> Continuous Delivery is the ability to get changes of all types—including new features, configuration changes, bug fixes and experiments—into production, or into the hands of users, safely and quickly in a sustainable way.

A lot of things go into delivering "continuously". These things can range from culture and behavior to specific automation. In this course, we're going to focus on deployment automation.

## Step 1: Configure a trigger based on labels

During the `on` step, we define what should cause this workflow to run. In this case, we want the workflow to run whenever a label is applied to the pull request.

### :keyboard: Activity: Configure the workflow trigger based on an a label being added

1. Edit this file
2. Change the name of the directory `CHANGETHIS` to `workflows`, so the title of this file with the path is `.github/workflows/staging-workflow.yml`
3. Edit the contents of this file to trigger on a label

Your result should look like this:

```yml
name: Staging deployment

on: 
  pull_request:
    types: [labeled]

jobs:
  build:
    runs-on: ubuntu-latest    
```