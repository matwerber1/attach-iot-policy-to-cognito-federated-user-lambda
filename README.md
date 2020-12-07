# attach-iot-policy-to-cognito-federated-user-lambda


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
