# Environment Variables

### Access keys for IAM Users

To automate the authentication, AWS recommends using a process called [IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html). By creating a specific key for a purpose or individual, specific scopes can be specified and access can be tracked.

When you create an access key, the key pair is active by default, and you can use the pair right away. You will be adding the following two secrets to this repository:

- **AWS_ACCESS**:  This serves as the user identifying token. "Access key ID"
- **AWS_SECRET**: This represents the secret key value pair that's like a password. It's under "Secret access key".

## Step 6: Create and store environment variables

### :keyboard: Activity: Create your AWS IAM secrets and enter them in this repository

1. Add the tokens to this repository with the token name `AWS_SECRET_KEY`
2. Once you are done, confirm here by commenting anything in this pull request
3. Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/
4. In the navigation pane, choose **Users**
5. Create a new user with **programmatic access**
6. When setting permissions, search for and select **AmazonS3FullAccess**
7. Use a tag that will identify this token pair, like **Deployment Learning Lab**
8. **Download .csv file** to save the access key ID and secret access key to a CSV file on your computer
  - Store the file in a secure location
    - ⚠️ _You will not have access to the secret access key again after this dialog box closes_
  - After you download the CSV file, choose **Close**
9. Save the _Access key ID_ as a secret, named `AWS_ACCESS_KEY` in the **Settings > Secrets**
10. Save the _Secret access key_ as a secret, named `AWS_SECRET_KEY` in the **Settings > Secrets**
11. Once you are done, confirm here by commenting anything in this pull request
