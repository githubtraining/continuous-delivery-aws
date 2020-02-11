# Azure configuration

GitHub Actions is cloud agnostic, so any cloud will work. We'll show how to deploy to Azure in this course.

### Azure resources

We'll use the following Azure resources in this course:
- A [web app](https://docs.microsoft.com/en-us/azure/app-service/overview) is how we'll be deploying our application to Azure.
- A [resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview#resource-groups) is a collection of resources, like web apps and virtual machines (VMs).
- An [App Service plan](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans) is what runs our web app and manages the billing (our app should run for free).

Through the power of GitHub Actions, we can create, configure, and destroy these resources through our workflow files. 

## Step 5: Spin up, configure, and destroy Azure resources

### :keyboard: Activity: Use your workflow file to configure your cloud resources

1. Edit the `.github/CHANGETHIS/spinup-destroy.yml` file on this branch, or [use this quick link]({{ repoUrl }}/edit/azure-configuration/.github/CHANGETHIS/spinup-destroy.yml?). _(We recommend opening the quick link in another tab.)_
1. Rename the files to `.github/workflows/spinup-destroy.yml`
1. Change the value of the `AZURE_WEBAPP_NAME:` to `{{ user.login }}-ttt-app`
1. Commit your changes.

The file should look like this:

```yaml
name: Configure Azure environment

on: 
  pull_request:
    types: [labeled]

env:
  PACKAGES_TOKEN: {% raw %}${{secrets.PACKAGES_TOKEN}}{% endraw %}
  AZURE_RESOURCE_GROUP: cd-with-actions
  AZURE_APP_PLAN: actions-ttt-deployment
  AZURE_LOCATION: '"Central US"'
  #################################################
  ### USER PROVIDED VALUES ARE REQUIRED BELOW   ###
  #################################################
  #################################################
  ### REPLACE USERNAME WITH GH USERNAME         ###
  AZURE_WEBAPP_NAME: USERNAME-ttt-app
  #################################################

jobs:
  setup-up-azure-resources:
    runs-on: ubuntu-latest

    if: contains(github.event.pull_request.labels.*.name, 'spin up environment')

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Azure login
        uses: azure/login@v1
        with:
          creds: {% raw %}${{ secrets.AZURE_CREDENTIALS }}{% endraw %}

      - name: Create Azure resource group
        if: success()
        run: |
          {% raw %}az group create --location ${{env.AZURE_LOCATION}} --name ${{env.AZURE_RESOURCE_GROUP}} --subscription ${{secrets.AZURE_SUBSCRIPTION_ID}}{% endraw %}

      - name: Create Azure app service plan
        if: success()
        run: |
          {% raw %}az appservice plan create --resource-group ${{env.AZURE_RESOURCE_GROUP}} --name ${{env.AZURE_APP_PLAN}} --is-linux --sku F1 --subscription ${{secrets.AZURE_SUBSCRIPTION_ID}}{% endraw %}

      - name: Create webapp resource
        if: success()
        run: |
          {% raw %}az webapp create --resource-group ${{ env.AZURE_RESOURCE_GROUP }} --plan ${{ env.AZURE_APP_PLAN }} --name ${{ env.AZURE_WEBAPP_NAME }}  --deployment-container-image-name nginx --subscription ${{secrets.AZURE_SUBSCRIPTION_ID}}{% endraw %}

      - name: Configure webapp to use GitHub Packages
        if: success()
        run: |
          {% raw %}az webapp config container set --docker-custom-image-name nginx --docker-registry-server-password ${{secrets.GITHUB_TOKEN}} --docker-registry-server-url https://docker.pkg.github.com --docker-registry-server-user ${{github.actor}} --name ${{ env.AZURE_WEBAPP_NAME }} --resource-group ${{ env.AZURE_RESOURCE_GROUP }} --subscription ${{secrets.AZURE_SUBSCRIPTION_ID}}{% endraw %}

  destroy-azure-resources:
    runs-on: ubuntu-latest

    if: contains(github.event.pull_request.labels.*.name, 'destroy environment')

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Azure login
        uses: azure/login@v1
        with:
          creds: {% raw %}${{ secrets.AZURE_CREDENTIALS }}{% endraw %}

      - name: Destroy Azure environment
        if: success()
        run: |
          {% raw %}az group delete --name ${{env.AZURE_RESOURCE_GROUP}} --subscription ${{secrets.AZURE_SUBSCRIPTION_ID}} --yes{% endraw %}
```

I'll respond when I detect a commit on this branch.