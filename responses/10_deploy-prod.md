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

{% if preferences.cloud == 'aws' %}
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
          AWS_ACCESS_KEY: {% raw %}${{ secrets.AWS_ACCESS_KEY }}{% endraw %}
          AWS_SECRET_KEY: {% raw %}${{ secrets.AWS_SECRET_KEY }}{% endraw %}

```
{% else %}
```yml
name: Production deployment

on: 
  push:
    branches:
      - master

env:
  DOCKER_IMAGE_NAME: hectorsector-tic-tac-toe # Must not exist as a package associated with a different repo!
  IMAGE_REGISTRY_URL: docker.pkg.github.com
  #################################################
  ### USER PROVIDED VALUES ARE REQUIRED BELOW   ###
  #################################################
  #################################################
  ### REPLACE USERNAME WITH GH USERNAME         ###
  AZURE_WEBAPP_NAME: hectorsector-ttt-app
  #################################################

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

  Build-Docker-Image:
    runs-on: ubuntu-latest
    needs: build
    name: Build image and store in GitHub Packages
    steps:
      - name: Checkout
        uses: actions/checkout@v1

      - name: Download built artifact
        uses: actions/download-artifact@master
        with:
          name: webpack artifacts
          path: public

      - name: create image and store in Packages
        uses: mattdavis0351/actions/docker-gpr@1.3.0
        with:
          repo-token: {% raw %}${{secrets.GITHUB_TOKEN}}{% endraw %}
          image-name: {% raw %}${{env.DOCKER_IMAGE_NAME}}{% endraw %}

  Deploy-to-Azure:
    runs-on: ubuntu-latest
    needs: Build-Docker-Image
    name: Deploy app container to Azure
    steps:
      - name: "Login via Azure CLI"
        uses: azure/login@v1
        with:
          creds: {% raw %}${{ secrets.AZURE_CREDENTIALS }}{% endraw %}

      - uses: azure/docker-login@v1
        with:
          login-server: {% raw %}${{env.IMAGE_REGISTRY_URL}}{% endraw %}
          username: {% raw %}${{ github.actor }}{% endraw %}
          password: {% raw %}${{ secrets.GITHUB_TOKEN }}{% endraw %}

      - name: Deploy web app container
        uses: azure/webapps-container-deploy@v1
        with:
          app-name: {% raw %}${{env.AZURE_WEBAPP_NAME}}{% endraw %}
          images: {% raw %}${{env.IMAGE_REGISTRY_URL}}/${{ github.repository }}/${{env.DOCKER_IMAGE_NAME}}:${{ github.sha }}{% endraw %}

      - name: Azure logout
        run: |
          az logout
```
{% endif %}