# Welcome to the course!

We'll learn how to create a workflow that enables Continuous Delivery. You'll:
- create a workflow to deploy to staging based on a label
- create a workflow to deploy to production based on merging to master

Before you start, you should be familiar with GitHub and Continuous Integration. If you aren't sure where to start, you may want to check out these two Learning Lab courses:

- [Introduction to GitHub](https://lab.github.com/githubtraining/introduction-to-github)
- [Continuous Integration with GitHub Actions](https://lab.github.com/githubtraining/set-up-continuous-integration-with-github-actions)

### What is Continuous Delivery?

[Martin Fowler](https://martinfowler.com/bliki/ContinuousDelivery.html) defined Continuous Delivery very simply in a 2013 post as follows:

> Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time.

A lot of things go into delivering "continuously". These things can range from culture and behavior to specific automation. In this course, we're going to focus on deployment automation.

### Kicking off deployments

Every deployment is kicked off by some trigger. Engineers at many companies, like at GitHub, typically use a ChatOps command as a trigger. The trigger itself isn't incredibly important. In our use case, we'll use labels. When someone applies a "stage" label to a pull request, that'll be our indicator that we'd like to deploy our application to a staging environment.

## Step 1: Configure a trigger based on labels

In a GitHub Actions workflow, the `on` step defines what causes the workflow to run. In this case, we want the workflow to run whenever a label is applied to the pull request.

### :keyboard: Activity: Configure the workflow trigger based on an a label being added

1. Edit this file
2. Change the name of the directory `CHANGETHIS` to `workflows`, so the title of this file with the path is `.github/workflows/deploy-staging.yml`
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
