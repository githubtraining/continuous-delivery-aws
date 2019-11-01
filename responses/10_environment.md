# Choosing an environment

Just like with the other workflow, we will need to specify an environment for AWS.

## Step 10: Choose the environment for AWS

### :keyboard: Choose an Ubuntu environment for the production deployment

```yml
name: Production deployment

on: 
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-latest
```