# Workflow steps

So far, the workflow knows what the trigger is and what environment to run in. But, what exactly is supposed to run? The "steps" section of this workflow specify what actions to be run in the Ubuntu environment when new labels are added.

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
          AWS_ACCESS_KEY: ${{ secrets.AWS_ACCESS_KEY }}
          AWS_SECRET_KEY: ${{ secrets.AWS_SECRET_KEY }}

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
          repo-token: ${{ secrets.GITHUB_TOKEN }}
          image-name: tic-tac-toe
```