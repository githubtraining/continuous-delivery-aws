# Job conditionals

GitHub Actions features powerful controls for when to execute jobs and the steps within them. One of these controls is `if`, which allows you run a job only when a specific condition is met. See [`jobs.<job_id>.if` in _Workflow syntax for GitHub Actions_](https://help.github.com/en/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idif) for more information.

### Using information within GitHub

Workflows are part of the GitHub ecosystem, so each workflow run gives you access to a rich set of data that you can use to take fine-grained control.

We'd like to run our workflow on a specific label name, suppose that it's **peacock**. We can run the workflow based on that labels name with the following single line that packs a punch: `if: contains(github.event.pull_request.labels.*.name, 'peacock')`. Here's how it works:
- [`if:`](https://help.github.com/en/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idif) is the conditional that will decide if the job will run
- [`contains()`](https://help.github.com/en/github/automating-your-workflow-with-github-actions/contexts-and-expression-syntax-for-github-actions#contains) is a function that allows us to determine if a value like say, a label named `"peacock"`, is contained within a set of values like say, a label array `["bug", "stage", "peacock", "feature request"]`
- `github.event.pull_request.labels` is specifically accessing the set of labels that triggered the workflow to run. It does this by accessing the [`github` object](https://help.github.com/en/github/automating-your-workflow-with-github-actions/contexts-and-expression-syntax-for-github-actions#github-context), and the [`pull_request` event](https://help.github.com/en/github/automating-your-workflow-with-github-actions/events-that-trigger-workflows#pull-request-event-pull_request) that triggered the workflow.
- `github.event.pull_request.labels.*.name` uses [object filters](https://help.github.com/en/github/automating-your-workflow-with-github-actions/contexts-and-expression-syntax-for-github-actions#github-context) to filter out all information about the labels, like their color or description, and lets us focus on just the label names. 

## Step 2: Trigger a job on specific labels

Let's put all this together to run our job only when a labeled named "stage" is applied to the pull request.

### :keyboard: Activity: Add a conditional to select from labels

{% if preferences.cloud == 'aws' %}
1. Edit the `deploy-staging.yml` file on this branch, or [use this quick link]({{ repoUrl }}/edit/staging-workflow/.github/workflows/deploy-staging.yml?) _(We recommend opening the quick link in another tab)_
2. Edit the contents of the file to add a conditional that filters the `build` job using a label called "stage".

Your results should look like this:

```yml
name: Stage the app

on: 
  pull_request:
    types: [labeled]

jobs:
  build:
    runs-on: ubuntu-latest

    if: contains(github.event.pull_request.labels.*.name, 'stage')
```
{% else %}
1. Edit the `deploy-staging.yml` file on this branch, or [use this quick link]({{ repoUrl }}/edit/staging-workflow/.github/workflows/deploy-staging.yml?) _(We recommend opening the quick link in another tab)_
1. Edit the contents of the file to add a block for environment variables before your jobs, as follows.
    ```yaml
    env:
      DOCKER_IMAGE_NAME: {{user.login}}-tic-tac-toe
      IMAGE_REGISTRY_URL: docker.pkg.github.com
      #################################################
      ### USER PROVIDED VALUES ARE REQUIRED BELOW   ###
      #################################################
      #################################################
      ### REPLACE USERNAME WITH GH USERNAME         ###
      AZURE_WEBAPP_NAME: {{user.login}}-ttt-app
      #################################################
    ```
1. Edit the contents of the file to add a conditional that filters the `build` job using a label called "stage".

Your results should look like this:

```yml
name: Stage the app

on: 
  pull_request:
    types: [labeled]

  env:
    DOCKER_IMAGE_NAME: {{user.login}}-tic-tac-toe
    IMAGE_REGISTRY_URL: docker.pkg.github.com
    #################################################
    ### USER PROVIDED VALUES ARE REQUIRED BELOW   ###
    #################################################
    #################################################
    ### REPLACE USERNAME WITH GH USERNAME         ###
    AZURE_WEBAPP_NAME: {{user.login}}-ttt-app
    #################################################

jobs:
  build:
    runs-on: ubuntu-latest

    if: contains(github.event.pull_request.labels.*.name, 'stage')
```

{% endif %}