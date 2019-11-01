# Choosing deployment environments

We will be working with AWS for the deployment environment. AWS will do the work of creating the environment, but first, we need to tell it what we need. That happens in the `environment` section of the workflow file.

### What are the options?

You may want to choose a different environment based on your application. You can read more about [virtual environments for GitHub Actions](https://help.github.com/en/github/automating-your-workflow-with-github-actions/virtual-environments-for-github-actions) on GitHub Help.

## Step 2: Choose the environment for AWS

For our `Node.js` application, we will be using a basic Ubuntu environment.

### :keyboard: Activity: Choose the Ubuntu environment for our app

```yml
name: Staging deployment

on: 
  pull_request:
    types: [labeled]

jobs:
  build:
    if: contains(github.event.pull_request.labels.*.name, 'stage')

    runs-on: ubuntu-latest
```