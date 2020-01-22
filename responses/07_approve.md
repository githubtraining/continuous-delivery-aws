# AWS Configuration files

To deploy successfully to our S3 bucket on AWS, we need a few special configuration files.

### `aws-config.yml`

The `aws-config.yml` file is needed with the `Deploy to AWS` Action that we're using. **How did we know that it's needed?** In the [documentation for the GitHub action](https://github.com/github/deploy-nodejs), there are specific instructions about including this file, and where it needs to sit within the repository.

Whenever you're using a GitHub Action, it's important to read the documentation. There may be details like what secrets or other template files are required for the Action to work as expected.

### `sam-template.yml`

This file is a bit trickier. The template `aws-config.yml` file that was documented with the action has a placeholder for this template, but doesn't specify what we should do.

This file is specific to deploying a serverless application to AWS. Specifically, an AWS SAM template tells AWS how to set up the application's architecture. Read more about it in [_AWS SAM Template Concepts_](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template-basics.html) in the AWS documentation.

In our case, we created the `sam-template.yml` for you. It contains information that's specific about the application's endpoints and structure.

## Step 6: Approve the pull request

I've requested your approval on this pull request. Once you approve this, I will merge.

### :keyboard: Activity: Approve pull request adding `aws-config.yml` and `sam-template.yml`

1. Approve this pull request
