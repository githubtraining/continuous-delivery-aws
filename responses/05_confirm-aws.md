# AWS Configuration - S3 Buckets

GitHub Actions is cloud agnostic, so any cloud will work. We'll show how to deploy to AWS in this course.

### S3 Buckets

Amazon S3 Buckets are a very flexible type of data storage -- they can be configured to work in many different types of ways. They're popular for their security, scalability, and dependability. Our S3 Bucket will store our application, both in staging and in production.

## Step 5: Confirm AWS S3 configuration

### :keyboard: Activity: Create an AWS account by the following specifications, and confirm here

1. Navigate to the [Amazon S3](https://s3.console.aws.amazon.com/s3/home) service and click on **Create bucket**.
    - See [_Create a Bucket_](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) on AWS documentation for the most detailed instructions.
1. Name the bucket whatever you'd like, and jot the name down
1. Select a region, and jot it down. You'll need it later. These examples use **us-west-2**. If you'd like to choose another region, make sure to update the `aws-config.yml` file to match.
1. For all other options, accept the defaults.
1. In `.github/aws-config.yml` on this branch, change the value of `s3_bucket:` to match your chosen bucket name. 
1. In the same file, confirm that the `region:` value matches your chosen region for the S3 bucket.
1. Commit your changes.

I'll respond when I detect a commit on this branch.