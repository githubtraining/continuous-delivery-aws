The deployment may take a few moments but you've done the right thing. Once the deployment is successful, you'll see green check marks for each run, and you'll see a URL for your deployment.

{% if preferences.cloud == 'aws' %}
![a screenshot of the Actions logs showing a completed deployment with an Output section and a staging URL](https://user-images.githubusercontent.com/16547949/68046896-6aa42a80-fcb3-11e9-9927-f493201167dc.png)
{% else %}
![a screenshot of the Actions logs showing a completed deployment with an Output section and a staging URL](https://user-images.githubusercontent.com/16547949/75822370-dc79a700-5d6d-11ea-8840-0a792f0639a5.png)
{% endif %}

You can wait for the deployment, or move on to the next steps in the [next pull request]({{ url }}). 

If you'd like to come back and merge this once their other workflow is done, just approve this pull request and merge. :tada: