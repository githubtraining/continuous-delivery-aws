# Choosing deployment environments

We will be working with AWS for the deployment environment. AWS will do the work of creating the environment, but first, we need to tell it what we need. That happens in the `environment` section of the workflow file.

### What are the options?

You may want to choose a different environment based on your application. For example, if you are building an application to run on an Android phone, the environment may be X. If the app will be for a Mac, you may choose X. Options of environments may be.... 

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