{% if preferences.cloud == 'azure' %}
Now that you've applied the proper label, let's wait for the GitHub Actions workflow to complete. When it's finished, you can confirm that your environment has been destroyed by visiting your app's URL, or by logging into the Azure portal to see it is not running.
{% else %}
Thanks for letting me know you've cleaned up your AWS environment. You can confirm that your environment has been destroyed by visiting your app's URL, or by logging into the AWS portal to see it is not running.
{% endif %}

This course is now complete! I'll stop responding but the fun doesn't have to stop here. 

![celebrate](https://octodex.github.com/images/jetpacktocat.png)

Now...[what will you learn next]({{ host }}/courses)?
