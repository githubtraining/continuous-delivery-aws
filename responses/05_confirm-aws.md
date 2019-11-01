# AWS Configuration

For deployment, we will be using AWS. 

### S3 Buckets

### Access keys for IAM Users

To automate the authentication, AWS recommends using a process called [IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html). By creating a specific key for a purpose or individual, specific scopes can be specified and access can be tracked.

When you create an access key, the key pair is active by default, and you can use the pair right away. You will be adding the following two secrets to this repository:

- **AWS_ACCESS**:  This serves as the user identifying token. "Access key ID"
- **AWS_SECRET**: This represents the secret key value pair that's like a password. It's under "Secret access key".

## Step 5: Confirm AWS configuration

### :keyboard: Activity: Create an AWS account by the following specifications, and confirm here.

1. Create an account at [aws.amazon.com](https://aws.amazon.com/)
   - _This requires credit card information. If you'd like to continue with the course without an AWS account, Learning Lab will still respond, but none of the deployments will work._
2. [Create an S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)
   - The region needs to be the same as what is specified in the  `aws-config.yml` file in this pull request. :eyes: **For this exercise, choose us-west-2**. :eyes: If you'd like to choose another region, make sure to update the `aws-config.yml` file to match.
3. Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/
4. In the navigation pane, choose **Users**
5. Choose the name of the user whose access keys you want to manage, and then choose the Security credentials tab
6. In the Access keys section:
  - Choose **Create access key**
  - Then, choose **Download .csv file** to save the access key ID and secret access key to a CSV file on your computer
  - Store the file in a secure location
    - ⚠️_You will not have access to the secret access key again after this dialog box closes_
  - After you download the CSV file, choose **Close**
7. Save the `AWS_ACCESS` token in the **Settings > Secrets**
8. Save the `AWS_SECRET` token in the **Settings > Secrets**
9. Once you are done, confirm here by commenting anything in this pull request
