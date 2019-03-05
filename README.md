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

To delete your function do not forget to:

```
serverless remove
```

## Deploy with [KLR](https://github.com/triggermesh/knative-lambda-runtime)

You will need a [Knative](https://github.com/knative) installation.

First install the KLR Python template like so:

```
tm deploy buildtemplate -f https://raw.githubusercontent.com/triggermesh/knative-lambda-runtime/master/python-3.7/buildtemplate.yaml
```

Then deploy the `handler.py` like so:

```
tm deploy service python-test -f . \
                              --build-template knative-python37-runtime \
                              --build-argument HANDLER=handler.endpoint \
                              --wait
```

This deployment will return something like:

```
...
Creating python-test function
Deployment started. Run "tm -n sebgoa describe service python-test" to see the details
Waiting for python-test ready state
Service python-test URL: http://python-test.sebgoa.k.triggermesh.io
```

And you will be able to curl the endpoint and get the same result as in AWS Lambda.

To delete the function:

```
tm delete services python-test
```

## Using your own private Docker registry

If you use `tm` without specifying a registry destination it will assume you are using `https://cloud.triggermesh.io` and use the internal Docker registry.

If you want to use your own registry, specify it in the build argument

```
tm deploy service python-test -f . \
                              --build-template knative-python37-runtime \
                              --build-argument IMAGE=index.docker.io/runseb/python-test
                              --build-argument HANDLER=handler.endpoint \
                              --wait
```

