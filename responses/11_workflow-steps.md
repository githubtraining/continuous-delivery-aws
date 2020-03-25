# Workflow steps

We'll add a final section to our production workflow that packages up our application in a Docker container and publishes it to GitHub Packages. This step is important for the traceability of your deployed artifacts.

We'll only use one new action here created by a GitHubber, which allows us to push a container to GitHub Packages. 
- `mattdavis0351/actions/docker-gpr`

All of this happens automatically once a pull request is merged!

## Step 10: Create the Docker image and push it to GitHub Packages

### :keyboard: Activity: Write the steps for the production deployment workflow

1. Edit the `deploy-prod.yml` file on this branch, or [use this quick link]({{ repoUrl }}/edit/production-deployment-workflow/.github/workflows/deploy-prod.yml?) _(We recommend opening the quick link in another tab)_
1. Add a job to your workflow as follows:
    ```yml
    Build-and-Push-Docker-Image:
    runs-on: ubuntu-latest
    needs: build
    name: Docker Build, Tag, Push
    steps:
      - name: Checkout
        uses: actions/checkout@v1

      - name: Download built artifact
        uses: actions/download-artifact@master
        with:
          name: webpack artifacts
          path: public

      - name: Build, Tag, Push
        uses: mattdavis0351/actions/docker-gpr@v1
        with:
          repo-token: {% raw %}${{ secrets.GITHUB_TOKEN }}{% endraw %}
          image-name: {{user.login}}-aws-ttt
    ```
1. Commit the workflow to this branch.

The complete workflow file should look like this:

```yml
name: Production deployment

on: 
  push:
    branches:
      - master

jobs:
  build:
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
        uses:  github/deploy-nodejs@master
        env:
          AWS_ACCESS_KEY: {% raw %}${{ secrets.AWS_ACCESS_KEY }}{% endraw %}
          AWS_SECRET_KEY: {% raw %}${{ secrets.AWS_SECRET_KEY }}{% endraw %}

  Build-and-Push-Docker-Image:
    runs-on: ubuntu-latest
    needs: build
    name: Docker Build, Tag, Push
    steps:
      - name: Checkout
        uses: actions/checkout@v1

      - name: Download built artifact
        uses: actions/download-artifact@master
        with:
          name: webpack artifacts
          path: public

      - name: Build, Tag, Push
        uses: mattdavis0351/actions/docker-gpr@v1
        with:
          repo-token: {% raw %}${{ secrets.GITHUB_TOKEN }}{% endraw %}
          image-name: {{user.login}}-aws-ttt
```
