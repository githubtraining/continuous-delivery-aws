# Different triggers

For the deployment to production, the trigger will be a merge to master.

## Step 9: Write the production deployment trigger

### :keyboard: Activity: Write the production deployment trigger on merge to master

1. First, change the `CHANGETHIS` directory to `workflows`
2. Make the file look like this 

```yml
name: Production deployment

on: 
  push:
    branches:
      - master
```