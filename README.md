# AWS Control Tower Customizations

This repository was forked from: https://github.com/aws-samples/aws-control-tower-config-customization
And is part of AWS blog post https://aws.amazon.com/blogs/mt/customize-aws-config-resource-tracking-in-aws-control-tower-environment/

Please refer to the blog for what this sample code does and how to use it.

## Deployment

This is deployed as a CF Stack in the Management account. 

## Customization 

We changed the following from the original blog post. 

1. Since we changed the code for the consumer lambda, we added it inlined in the CF template. 
   The original blog post had it as a separate file.
2. Instead of explicitly including resources to record config events on, we just disable it for certain resource types.
   
   See the code in the consumer lambda.

3. Custom lambda layer
   We use a custom lambda layer to include the current `boto3` library.
   See below how to build and deploy the layer

4. Manually added the boto3 layer to the consumer lambda via the AWS console.

## How to build boto3 layer

```bash
   mkdir boto3-layer
    cd boto3-layer
    python3 -m venv .venv
    source .venv/bin/activate
    pip install boto3
    cp -rp .venv/lib/python3.8/site-packages/* python/
    zip -r boto3-layer.zip python
    aws lambda publish-layer-version --layer-name python-boto3 --zip-file fileb://./boto3_requests.zip --profile 305497534492:AWSAdministratorAccess --region eu-central-1
```

Assign the layer to the consumer lambda via the AWS console.

## Notes

This CF template copies code from a AWS owned S3 bucket to our own S3 bucket and then uses that code for the lambdas.
We changed that insofar that we inlined the consumer code into the CF template.

