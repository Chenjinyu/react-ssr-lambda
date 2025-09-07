### Instruction 
https://aws.amazon.com/blogs/compute/building-server-side-rendering-for-react-in-aws-lambda/

#### AWS CDK 
AWS CDK is an infrastructure-as-code(IAC) framework that allows developers to define their cloud infrastrucutre using high-level programming lanuages such as TypeScript, Python and Java. 
With CDK, developers can write code that defines AWS resources such as EC2 instance, S3 buckets, and Lambda functions, and then use CDK to deploy and manage that infrastructure in a repeatable and predictable way. 

```bash
cd ./cdk
npm install
npm run build
cdk bootstrap
cdk deploy SSRApiStack --outputs-file ../simple-ssr/src/config.json

cd ../simple-ssr
npm install
npm run build-all
cd ../cdk
cdk deploy SSRAppStack --parameters mySiteBucketName=<your bucket name>
```

1. When run `npm run build` for a TypeScript project and the TypeScript compiler (`tsc`) converts the TypeScript code to JavaScript code and generates a source map file(`*.js.map`) that maps the JavaScript code back to the original TypeScript code. This is useful for debugging TypeScript code in the browser or in a Node.js runtime environment.

If you don't want to include the sourceMappingURL directive in your JavaScript files, you can disable it by setting the sourceMap option to false in your TypeScript compiler configuration file (tsconfig.json). Here is an example of how to disable the source map file generation:

```json
{
  "compilerOptions": {
    "sourceMap": false,
    // other options...
  },
  // other configuration...
}
```

The data:application/json;base64,... syntax in the sourceMappingURL is actually a data URL, which encodes the source map as a base64-encoded string and includes it directly in the JavaScript file. This is a way to inline the source map in the JavaScript file and avoid the need for a separate source map file.

When you see a sourceMappingURL that looks like this:
```shell
//# sourceMappingURL=data:application/json;base64,...
```
It means that the source map is included inline in the JavaScript file itself. This is often done for smaller projects or libraries where it's not worth the overhead of generating a separate source map file.

**Note: Keep in mind that using a separate source map file will increase the number of HTTP requests that your application needs to make, but it can make debugging easier because you can inspect the original TypeScript code directly in the browser's developer tools or in a Node.js runtime environment.**


2. When run `cdk bootstrap` command is used to set up your AWS account and **prepare** it for deploying CDK apps. it doesn't directly "know" the CDK code for your AWS CloudFormation templates. Instead, it creates a special AWS CloudFormation stack called the "CDKToolkit" stack in your AWS account.

When you deploy your CDK app using cdk deploy, the CDK toolkit stack is used to manage the deployment of your app. The CDK toolkit stack includes resources that are needed by the CDK to deploy your app, such as an Amazon S3 bucket for storing app assets and artifacts, and an IAM role that grants the CDK permissions to deploy resources in your AWS account.


If you don't explicitly specify an IAM role in your AWS CDK code, the AWS CDK will automatically create an IAM role for your app during deployment.

By default, when you deploy an AWS CDK app, the AWS CDK will create an AWS CloudFormation stack that includes all of the resources required by your app. This includes any AWS resources that you define in your AWS CDK code, such as an Amazon S3 bucket, an AWS Lambda function, or an Amazon DynamoDB table.

When the AWS CDK creates a resource that requires an IAM role, such as an AWS Lambda function, it will automatically create an IAM role for that resource if you don't explicitly specify one in your AWS CDK code. The AWS CDK will assign the minimum required permissions to the IAM role based on the resource's configuration.

However, if you want to explicitly specify an IAM role in your AWS CDK code, you can do so using the role property of the resource's constructor. This allows you to define a custom IAM role with specific permissions for your resource.

So, to answer your question, if you don't specify an IAM role in your AWS CDK code, the AWS CDK will create an IAM role for your app during deployment, if needed.



output after the `cdk deploy SSRAppStack`
```shell
SSRAppStack.Bucket = react-ssr-lambda-demo
SSRAppStack.CFURL = https://d3dnju9inbcdxg.cloudfront.net
SSRAppStack.LambdaEdgeSSRURL = https://d3dnju9inbcdxg.cloudfront.net/edgessr
SSRAppStack.LambdaSSRURL = https://d3dnju9inbcdxg.cloudfront.net/ssr
SSRAppStack.SSRAPIURL = https://8l66948d83.execute-api.us-east-1.amazonaws.com/prod/
SSRAppStack.ssrEndpoint02F5615F = https://8l66948d83.execute-api.us-east-1.amazonaws.com/prod/
Stack ARN:
arn:aws:cloudformation:us-east-1:674415394971:stack/SSRAppStack/9167de40-e347-11ed-8ff3-0a69ccf5670f
```