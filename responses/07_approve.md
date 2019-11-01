# AWS Configuration files

To deploy successfully to our S3 bucket on AWS, we need a few special configuration files.

### `aws-config.yml`

The `aws-config.yml` file is needed with the `Deploy to AWS` Action that we're using. **How did we know that it's needed?** In the [documentation for the GitHub action](https://github.com/github/deploy-nodejs), there are specific instructions about including this file, and where it needs to sit within the repository.

Whenever you're using a GitHub Action, it's important to read the documentation. There may be details like what secrets or other template files are required for the Action to work as expected.

### `sam-template.yml`

This file is a bit trickier. The template `aws-config.yml` file that was documented with the action has a placeholder for this template, but doesn't specify what we should do.

In our case, we created the `sam-template.yml` for you. It contains information that's specific about the application source code in this repository. When we tell AWS to deploy, it wonders "Deploy _what_?". This file communicates which files should be deployed, and how, within our S3 bucket on AWS.

## Step 7: Approve the pull request

I've requested your approval on this pull request. Once you approve this, I will merge.

### :keyboard: Activity: Approve pull request adding `aws-config.yml` and `sam-template.yml`

1. Approve this pull request