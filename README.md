# attach-iot-policy-to-cognito-federated-user-lambda

The function parses the event input from API Gateway to determine the user's federated identity ID and user pool ID, attaches an IoT policy to the user, and updates the user's user pool attributes to set `ioTPolicyIsAttached = true`. The client-side app inspect this user attribute value when the user signs in (or attempts to use AWS IoT) to determine whether or not this function needs to be invoked first.

## Prerequisites

1. This Lambda function must be invoked by a Cognito Federated User via API Gateway. 

2. The API invoking this function must be configured to use AWS IAM authentication.

3. Your Cognito User Pool must have a custom boolean attribute named `iotPolicyIsAttached`. Or, you can use a different attribute name as long as you update the code.

4. You must have an existing AWS IoT Policy that you want to attach to your users. Update the `IOT_POLICY` constant in the code to match your policy name. 

## Example function response / output logs

```
Response:
{
  "statusCode": 200,
  "headers": {
    "Access-Control-Allow-Origin": "*"
  },
  "body": "{\n  \"msg\": \"IoT Policy amplify-toolkit-iot-message-viewer attached to Cognito federated identity ID us-west-2:XXXXX-XXXXXX-XXXXXX-XXXXX\"\n}"
}

Request ID:
"c987b3cd-cdeb-4f42-b275-0f7d11cd50dd"

Function logs:
START RequestId: c987b3cd-cdeb-4f42-b275-0f7d11cd50dd Version: $LATEST
2020-12-07T19:59:40.496Z	c987b3cd-cdeb-4f42-b275-0f7d11cd50dd	INFO	Attaching amplify-toolkit-iot-message-viewer IoT policy to federated ID us-west-2:XXXXX-XXXXXX-XXXXXX-XXXXX...
2020-12-07T19:59:40.904Z	c987b3cd-cdeb-4f42-b275-0f7d11cd50dd	INFO	Extracting Cognito user ID (aka 'sub') from auth string cognito-idp.us-west-2.amazonaws.com/us-west-2-XXXXXX,cognito-idp.us-west-2.amazonaws.com/us-west-2-XXXXXXXX:CognitoSignIn:1e10ff5b-XXXX-XXXXX-XXXXX-XXXXXXXXXXX...
2020-12-07T19:59:40.904Z	c987b3cd-cdeb-4f42-b275-0f7d11cd50dd	INFO	Calling Cognito.listUsers() to find username from user ID 1e10ff5b-XXXX-XXXXX-XXXXX-XXXXXXXXXXX...
2020-12-07T19:59:41.563Z	c987b3cd-cdeb-4f42-b275-0f7d11cd50dd	INFO	Username is mat@XXXXXX.com...
2020-12-07T19:59:41.601Z	c987b3cd-cdeb-4f42-b275-0f7d11cd50dd	INFO	Setting user attribute custom:iotPolicyIsAttached to "true"...
END RequestId: c987b3cd-cdeb-4f42-b275-0f7d11cd50dd
REPORT RequestId: c987b3cd-cdeb-4f42-b275-0f7d11cd50dd	Duration: 1316.80 ms	Billed Duration: 1317 ms	Memory Size: 128 MB	Max Memory Used: 89 MB	Init Duration: 430.19 ms	
```

## Example Event from API Gateway

When API Gateway is configured with IAM authentication and a Cognito Federated Identity Pool identity invokes the API, the event that is passed to Lambda looks like this:

```json
{
    "resource": "/demo",
    "path": "/demo",
    "httpMethod": "GET",
    "headers": {
        "Accept": "application/json, text/plain, ",
        "Accept-Encoding": "gzip, deflate, br",
        "Accept-Language": "en-US,en;q=0.9",
        "CloudFront-Forwarded-Proto": "https",
        "CloudFront-Is-Desktop-Viewer": "true",
        "CloudFront-Is-Mobile-Viewer": "false",
        "CloudFront-Is-SmartTV-Viewer": "false",
        "CloudFront-Is-Tablet-Viewer": "false",
        "CloudFront-Viewer-Country": "US",
        "Host": "ghl9weghc7.execute-api.us-west-2.amazonaws.com",
        "origin": "http://localhost:3000",
        "Referer": "http://localhost:3000/",
        "sec-fetch-dest": "empty",
        "sec-fetch-mode": "cors",
        "sec-fetch-site": "cross-site",
        "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.66 Safari/537.36",
        "Via": "2.0 1ec2938341958d70d56193d709c89def.cloudfront.net (CloudFront)",
        "X-Amz-Cf-Id": "UeyJVG96oTb8lrvXp0lns2K-xmJGBH5wgXgW-fIqH8ziKBtuv_8XZg==",
        "x-amz-date": "20201205T010327Z",
        "x-amz-security-token": "XXXXXXX",
        "X-Amzn-Trace-Id": "Root=1-5fcadc5f-787931785dea2be57e96c23c",
        "X-Forwarded-For": "73.140.151.157, 64.252.140.92",
        "X-Forwarded-Port": "443",
        "X-Forwarded-Proto": "https"
    },
    "multiValueHeaders": {
        "Accept": [
            "application/json, text/plain,"
        ],
        "Accept-Encoding": [
            "gzip, deflate, br"
        ],
        "Accept-Language": [
            "en-US,en;q=0.9"
        ],
        "CloudFront-Forwarded-Proto": [
            "https"
        ],
        "CloudFront-Is-Desktop-Viewer": [
            "true"
        ],
        "CloudFront-Is-Mobile-Viewer": [
            "false"
        ],
        "CloudFront-Is-SmartTV-Viewer": [
            "false"
        ],
        "CloudFront-Is-Tablet-Viewer": [
            "false"
        ],
        "CloudFront-Viewer-Country": [
            "US"
        ],
        "Host": [
            "ghl9weghc7.execute-api.us-west-2.amazonaws.com"
        ],
        "origin": [
            "http://localhost:3000"
        ],
        "Referer": [
            "http://localhost:3000/"
        ],
        "sec-fetch-dest": [
            "empty"
        ],
        "sec-fetch-mode": [
            "cors"
        ],
        "sec-fetch-site": [
            "cross-site"
        ],
        "User-Agent": [
            "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.66 Safari/537.36"
        ],
        "Via": [
            "2.0 1ec2938341958d70d56193d709c89def.cloudfront.net (CloudFront)"
        ],
        "X-Amz-Cf-Id": [
            "UeyJVG96oTb8lrvXp0lns2K-xmJGBH5wgXgW-fIqH8ziKBtuv_8XZg=="
        ],
        "x-amz-date": [
            "20201205T010327Z"
        ],
        "x-amz-security-token": [
            "XXXXXXX"
        ],
        "X-Amzn-Trace-Id": [
            "Root=1-5fcadc5f-787931785dea2be57e96c23c"
        ],
        "X-Forwarded-For": [
            "73.140.151.157, 64.252.140.92"
        ],
        "X-Forwarded-Port": [
            "443"
        ],
        "X-Forwarded-Proto": [
            "https"
        ]
    },
    "queryStringParameters": null,
    "multiValueQueryStringParameters": null,
    "pathParameters": null,
    "stageVariables": null,
    "requestContext": {
        "resourceId": "t1ltv4",
        "resourcePath": "/demo",
        "httpMethod": "GET",
        "extendedRequestId": "XDdfAGTIvHcF37Q=",
        "requestTime": "05/Dec/2020:01:03:27 +0000",
        "path": "/dev/demo",
        "accountId": "544941453660",
        "protocol": "HTTP/1.1",
        "stage": "dev",
        "domainPrefix": "ghl9weghc7",
        "requestTimeEpoch": 1607130207917,
        "requestId": "3e657bee-a83f-455e-aa1f-89d1bc2dcef4",
        "identity": {
            "cognitoIdentityPoolId": "us-west-2:5214f2db-370c-456c-8c4a-XXXXXXXXX",
            "accountId": "XXXXXXXXXXXX",
            "cognitoIdentityId": "us-west-2:d625c9c7-9a79-483c-bb4a-XXXXXXXXX",
            "caller": "AROAX5YIKWFOA7TX3PTGF:CognitoIdentityCredentials",
            "sourceIp": "73.140.151.157",
            "principalOrgId": "o-5csdfze7w6",
            "accessKey": "ASIAX5YIKWFOLP33BIJ5",
            "cognitoAuthenticationType": "authenticated",
            "cognitoAuthenticationProvider": "cognito-idp.us-west-2.amazonaws.com/us-west-XXXXXXXX,cognito-idp.us-west-2.amazonaws.com/us-west-2-XXXXXXXXX:CognitoSignIn:1e10ff5b-642b-489a-894c-XXXXXXXXX",
            "userArn": "arn:aws:sts::544941453660:assumed-role/amplify-awstoolkit-dev-155847-authRole/CognitoIdentityCredentials",
            "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.66 Safari/537.36",
            "user": "AROAX5YIKWFOA7TX3PTGF:CognitoIdentityCredentials"
        },
        "domainName": "ghl9weghc7.execute-api.us-west-2.amazonaws.com",
        "apiId": "ghl9weghc7"
    },
    "body": null,
    "isBase64Encoded": false
}
```
