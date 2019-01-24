# KLR sample 

This is a repo with an example to showcase how [KLR](https://github.com/triggermesh/knative-lambda-runtime) works and how the same function deploys to AWS lambda

The function we are going to deploy is a Python function that returns the current time, see [`handler.py`](./handler.py)

## Deploy to AWS Lambda

To deploy to Lambda we use the serverless [framework](https://serverless.com) and write a `serverless.yaml` file then:

```
serverless deploy
```

At the end of the deployment you will be given a URL where you can invoke the function, something like:

```
...
Service Information
service: aws-python-simple-http-endpoint
stage: dev
region: us-east-1
stack: aws-python-simple-http-endpoint-dev
api keys:
  None
endpoints:
  GET - https://10sjc1yh84.execute-api.us-east-1.amazonaws.com/dev/ping
functions:
  currentTime: aws-python-simple-http-endpoint-dev-currentTime
layers:
  None
```

## Deploy with [KLR](https://github.com/triggermesh/knative-lambda-runtime)

