# amazon-apigateway-amazon-s3-upload

A solution to store an HTTP request body as a file into Amazon Simple Storage Service (S3), via Amazon API Gateway as an entrypoint.

For instance, this solution can be used as a webhook for third parties to push JSON data for further processing.

This solution will upload files into S3 in `<PREFIX>_<EPOCH>_<GUID><SUFFIX>` format.

## Architecture

![](design.drawio.png)

**Please note**
* This is a sample solution. Extend as required for your organisational requirements / governance / compliance purposes.
* In this example, an API key is used for authentication. More info [here](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-key-source.html).
* This solution is meant to be used with trusted third parties. The solution does not check the files for vulnerabilities.

**Defaults**

|Name|Default|
|--|--|
|`PREFIX`|PREFIX|
|`SUFFIX`|.json|

Ex:

`PREFIX_1622613486775_eeea3f06-93ac-46e7-9b15-3c319c6fda8a.json`


## Prerequisites

1. Ensure [AWS CLI](https://aws.amazon.com/cli/) is installed.
1. Ensure [SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) is installed.

## To deploy
1. Run command `sam build`
1. Run command `sam deploy --guided`
1. When asked, provide the information required.
1. Copy the `outputs` section. These will be required later.
## Test

### 1. Get the API Key value

First, let's get the API key value. Replace the `API Key` below, from the corresponding value in the `outputs` section, and run the following command:
`aws apigateway get-api-key --api-key <API Key> --include-value`. In the resulting response, copy the `value`. This value will be used below.

### 2. Call the endpoint

After replacing the `API Url` from the corresponding value in the `outputs` section, and the `API Key Value` below with value copied from above, run the following `CURL` command.
```
curl -iX POST '<API Url>' \
--header 'x-api-key: <API Key Value>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "message": "test"
}'
```

### 3. Validate the result

In the AWS Management Console, go to the S3 bucket (the name of the bucket will be in the `outputs` section). You will see a file in the S3 bucket with the following payload:
```
{
    "message": "test"
}
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

