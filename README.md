# Amazon CloudFront Lambda hook: how to rewrite source url

## 1. Go to Services -> IAM and create IAM User if needed
https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html

![aws cdn lambda rewrite rule 001](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/001.jpg)

![aws cdn lambda rewrite rule 002](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/002.jpg)

## 2. Go to Roles tab and add new role
https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html

![aws cdn lambda rewrite rule 003](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/003.jpg)

## 3. Add to permissions
```
{
    "Sid": "LambdaAll",
    "Effect": "Allow",
    "Action": [
        "cloudfront:*",
        "lambda:*"
    ],
    "Resource": "*"
}
```
![aws cdn lambda rewrite rule 004](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/004.jpg)

![aws cdn lambda rewrite rule 005](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/005.jpg)

## 4. Edit trust relationships
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "lambda.amazonaws.com",
          "edgelambda.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
![aws cdn lambda rewrite rule 006](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/006.jpg)

![aws cdn lambda rewrite rule 007](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/007.jpg)

## 5. Go to Services -> Lambda

![aws cdn lambda rewrite rule 008](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/008.jpg)

## 6. Create function

![aws cdn lambda rewrite rule 009](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/009.jpg)

## 7. Select your role (step 2)

![aws cdn lambda rewrite rule 010](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/010.jpg)

## 8. Write your function
```
exports.handler = (event, context, callback) => {
    // request object
    var request = event.Records[0].cf.request;

    // rewrite the url
    // alias 'at' for directory 'affilinet'
    if (request.uri.indexOf('/at/') != -1) {
        request.uri = request.uri.replace('/at/', '/affilinet/');
    }
    callback( null, request );
};
```
![aws cdn lambda rewrite rule 011](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/011.jpg)

## 9. Configure test events

![aws cdn lambda rewrite rule 012](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/012.jpg)

There are some useful templates

![aws cdn lambda rewrite rule 013](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/013.jpg)

## 10. Publish your code

![aws cdn lambda rewrite rule 014](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/014.jpg)

## 11. Add trigger CloudFront
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-add-triggers-lam-console.html

![aws cdn lambda rewrite rule 015](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/015.jpg)

Then configure the trigger add it and save changes
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-cloudfront-trigger-events.html

![aws cdn lambda rewrite rule 016](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/016.jpg)

To switch versions of your hook use Qualifiers menu

![aws cdn lambda rewrite rule 017](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/017.jpg)

## 12. Go to Services -> CloudFront

![aws cdn lambda rewrite rule 018](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/018.jpg)

And check you Distribution Status (wait until it changes to Deployed)

![aws cdn lambda rewrite rule 019](https://github.com/droganaida/aws-cdn-rewrite-url/blob/master/images/019.jpg)