# Workflow steps

So far, the workflow knows what the trigger is and what environment to run in. But, what exactly is supposed to run? The "steps" section of this workflow specify what actions to be run in the Ubuntu environment when new labels are added.

## Step 3: Write the steps for the staging workflow

We won't be going into detail on the steps of this workflow, but it would be a good idea to check them out. You'll see that we're adding steps using existing actions for:

- `actions/checkout`
- `Deploy to AWS`

### :keyboard: Activity: Write the steps for the staging deployment workflow

```yml
name: Staging deployment

on: 
  pull_request:
    types: [labeled]

jobs:
  build:
    if: contains(github.event.pull_request.labels.*.name, 'stage')

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1
      - name: npm install and build webpack
        run: |
          npm install
          npm run build
      - uses: actions/upload-artifact@master
        with:
          name: webpack artifacts
          path: public/

  deploy:
    name: Deploy Node.js app to AWS
    needs: build
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1

      - name: Download built artifact
        uses: actions/download-artifact@master
        with:
          name: webpack artifacts
          path: public

      - name: Deploy to AWS
        uses: docker://admiralawkbar/aws-nodejs:latest
        env:
          AWS_ACCESS_KEY: ${{ secrets.AWS_ACCESS_KEY }}
          AWS_SECRET_KEY: ${{ secrets.AWS_SECRET_KEY }}
```