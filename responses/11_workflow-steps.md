# Workflow steps

So far, the workflow knows what the trigger is and what environment to run in. But, what exactly is supposed to run? The "steps" section of this workflow specify what actions to be run in the Ubuntu environment when new labels are added.

With the staging deployment, we use `checkout` and `Deploy to AWS`. In this workflow, we use: 

- `actions/checkout@v1`
- `Deploy to AWS`

We also have a new section past the `deploy` section, called **Docker Build, Tag, Push**, or Build-and-Push-Docker-Image. This part of the workflow uses the action from another course. That action builds the code, tags the commit, and pushes a package to the GitHub Package Registry.

- `actions/checkout@v1`
- `actions/download-artifact@master`
- `mattdavis0351/actions/docker-gpr@v1`

All of this happens automatically once a pull request is merged!

## Step 11: Write the steps for the production workflow

### :keyboard: Activity: Write the steps for the production deployment workflow

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
        uses: docker://admiralawkbar/aws-nodejs:latest
        env:
          AWS_ACCESS_KEY: ${% raw %}{{ secrets.AWS_ACCESS_KEY }}{% endraw %}
          AWS_SECRET_KEY: ${% raw %}{{ secrets.AWS_SECRET_KEY }}{% endraw %}

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
          repo-token: ${% raw %}{{ secrets.GITHUB_TOKEN }}{% endraw %}
          image-name: tic-tac-toe
```