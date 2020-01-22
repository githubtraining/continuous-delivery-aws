# Workflow steps

So far, the workflow knows what the trigger is and what environment to run in. But, what exactly is supposed to run? The "steps" section of this workflow specifies actions and scripts to be run in the Ubuntu environment when new labels are added.

## Step 3: Write the steps for the staging workflow

We won't be going into detail on the steps of this workflow, but it would be a good idea to become familiar with the actions we're using. They are:

- [`actions/checkout`](https://github.com/actions/checkout)
- [`actions/upload-artifact`](https://github.com/actions/upload-artifact)
- [`actions/download-artifact`](https://github.com/actions/download-artifact)
- [`github/deploy-nodejs`](https://github.com/github/deploy-nodejs)

The course [_Using GitHub Actions for CI_](https://lab.github.com/githubtraining/github-actions:-continuous-integration) also teaches how to use most of these actions in details.

### :keyboard: Activity: Deploy a Node.js app to AWS for the first time

1. In a new tab, [create an AWS account](https://portal.aws.amazon.com/billing/signup) if you don't already have one.
    > Note: You may need a credit card to create an AWS account. If you're a student, you may also be able to take advantage of the [Student Developer Pack](https://education.github.com/pack) for access to AWS. If you'd like to continue with the course without an AWS account, Learning Lab will still respond, but none of the deployments will work.
1. **[Add a user](https://console.aws.amazon.com/iam/home?#/users$new?step=details)** in the IAM service with administrator permission.
    - For detailed instructions on adding users, see [_Creating an IAM User in Your AWS Account_](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) in the AWS docs.
1. In the confirmation screen, copy both the **Access key ID** and the **Secret access key** to a safe space. We'll use these in the next few steps as follows:
    - Access key ID ➡️ `AWS_ACCESS_KEY`
    - Secret access key ️️️ ➡️ `AWS_SECRET_KEY`
1. Back on GitHub, click on this repository's **[Secrets]({{ repoUrl }}/settings/secrets)** in the Settings tab.
1. Click **Add a new secret**
1. Name your new secret **AWS_ACCESS_KEY** and paste the value from the Access key ID generated on AWS.
1. Click **Add secret**.
1. Click **Add a new secret** again.
1. Name the second secret **AWS_SECRET_KEY** and paste the value from the Secret access key generated on AWS.
1. Click **Add secret**
2. Back in this pull request, edit the `.github/workflows/deploy-staging.yml` file to use a new action, or [use this quick link]({{ repoUrl }}/edit/staging-workflow/.github/workflows/deploy-staging.yml?) _(We recommend opening the quick link in another tab)_
    ```yml
    - name: Deploy to AWS
      uses: github/deploy-nodejs@master
      env:
        AWS_ACCESS_KEY: {% raw %}${{ secrets.AWS_ACCESS_KEY }}{% endraw %}
        AWS_SECRET_KEY: {% raw %}${{ secrets.AWS_SECRET_KEY }}{% endraw %}
    ```
If you'd like to copy the full workflow file, it should look like this:

```yml
name: Staging deployment

on: 
  pull_request:
    types: [labeled]

jobs:
  build:
    if: contains(github.event.pull_request.labels.*.name, 'stage')

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1
      - name: npm install and build webpack
        run: |
          npm install
          npm run build
      - uses: actions/upload-artifact@master
        with:
          name: webpack artifacts
          path: public/

  deploy:
    name: Deploy Node.js app to AWS
    needs: build
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1

      - name: Download built artifact
        uses: actions/download-artifact@master
        with:
          name: webpack artifacts
          path: public

      - name: Deploy to AWS
        uses: github/deploy-nodejs@master
        env:
          AWS_ACCESS_KEY: {% raw %}${{ secrets.AWS_ACCESS_KEY }}{% endraw %}
          AWS_SECRET_KEY: {% raw %}${{ secrets.AWS_SECRET_KEY }}{% endraw %}
```
