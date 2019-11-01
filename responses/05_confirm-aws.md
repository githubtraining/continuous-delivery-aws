# AWS Configuration - S3 Buckets

For deployment, we will be using AWS.

### S3 Buckets

Amazon S3 Buckets are containers. They're also a very flexible type of data storage- they can be configured to work in many different types of ways. They're popular for their security, scalability, and dependability. Our S3 Bucket will be the container that our application is deployed in, both in staging and in production.

## Step 5: Confirm AWS S3 configuration

### :keyboard: Activity: Create an AWS account by the following specifications, and confirm here

1. Create an account at [aws.amazon.com](https://aws.amazon.com/)
   - _This requires credit card information. If you'd like to continue with the course without an AWS account, Learning Lab will still respond, but none of the deployments will work._
2. [Create an S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)
   - If you aren't sure how to get there, you can search for `S3`.
   - Name the bucket `github-sam-test`
   - The region needs to be the same as what is specified in the  `aws-config.yml` file in this pull request. :eyes: **For this exercise, choose us-west-2**. :eyes: If you'd like to choose another region, make sure to update the `aws-config.yml` file to match.
   - For all other options, accept the defaults.
3. Confirm that you've created an S3 bucket by commenting anything in this pull request