## Nice work!

Now, we just have to wait for the deployment to occur, and for the package to be published to GitHub Packages. When it's completed, you should be able to see it in the [Packages]({{ repoUrl }}/packages) section of your repository. You can get the deployment URL in the [Actions]({{ repoUrl }}/actions) log, just like the staging URL.

# The cloud environment

Throughout the course you've spun up resources that, if left unattended, could incur billing or consume your free minutes from the cloud provider. Let's tear down those environments so that you can keep your minutes for more learning!

## Step 12: Clean up your environment

### :keyboard: Activity: Destroy any running resources so you don't incur charges

{% if preferences.cloud == 'azure' %}

1. Apply the **destroy environment** label to this pull request.
{% else %}
TODO: include instructions for tearing down AWS environment.
{% endif %}

I'll respond when you apply a label to this pull request. 