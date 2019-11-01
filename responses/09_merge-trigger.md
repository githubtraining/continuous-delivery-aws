# Different triggers

Deployments to production can be manual (like through a Chat Ops command), or automated (if, say, we trust our pull request review process and we've followed continuous integration practices).

We'll trigger a deployment to the production environment whenever something is committed to master. Our master branch is protected, so the only way for commits to appear on master is for a pull request to have been created and gone through the proper review process and merged.

## Step 9: Write the production deployment trigger

Let's create a new workflow that deals specifically with commits to master and handles deployments to prod.

### :keyboard: Activity: Write the production deployment trigger on merge to master

1. Rename the file in this pull request to `.github/workflows/deploy-prod.yml`.
1. Add a `push` trigger.
1. Add `branches` inside the push block.
1. Add `- master` inside the branches block.
1. Commit your changes to this branch. 

The file should look like this when you're finished:

```yml
name: Production deployment

on: 
  push:
    branches:
      - master
```