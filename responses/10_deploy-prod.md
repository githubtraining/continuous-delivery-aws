Great! The syntax you used tells GitHub Actions to only run that workflow when a commit is made to the master branch. 

# Deploying to production

Just like with the other workflow, we'll need to build our application and deploy to AWS using the same action as before because we are working with the same `Node.js` app. 

**Continuous delivery** is a concept that contains many behaviors and other, more specific concepts. One of those concepts is **test in production**. That can mean different things to different projects and different companies, and isn't a strict rule that says you are or aren't "doing CD".

In our case, we can match our production environment to be exactly like our staging environment. This minimizes opportunities for surprises once we deploy to production.

## Step 9: Complete the deployment to production workflow

### :keyboard: Commit the steps to the production workflow that allow you to deploy on merge to master

1. Edit the `deploy-prod.yml` file on this branch, or [use this quick link]({{ repoUrl }}/edit/production-deployment-workflow/.github/workflows/deploy-prod.yml?) _(We recommend opening the quick link in another tab)_
2. Add a `build` and `deploy` job to the workflow

It should look like the file below when you are finished. Note that not much has changed from our staging workflow, except for our trigger.

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
```
