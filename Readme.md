# Below AWS resources are used in the ASP.Net Core API 
* .net Core API with DynamoDB [Tutorial link](https://codewithmukesh.com/blog/crud-with-dynamodb-in-aspnet-core)
* S3 Bucket in API [Tutorial link](https://codewithmukesh.com/blog/working-with-aws-s3-using-aspnet-core)
* Serverless API in Lambda [Tutorial link](https://codewithmukesh.com/blog/hosting-aspnet-core-web-api-with-aws-lambda)

#### Follow below steps

## .net Core API with DynamoDB
- Create a dotnet 6 project using ASP.net Core WEB api template

**2 IAm Role Policy**
AmazonDynamoDBFullAccess
AmazonDynamoDBReadOnlyAccess
AWSLambdaDynamoDBExecutionRole
AWSLambdaInvocation-DynamoDB

**3 Create a user in IAM with user name *studentApi***
run below command
`aws configure --profile "studentApi"`

**4 update appsettings.json**
"AWS": {
  "Profile": "studentApi",
  "Region": "ap-south-1"
}

**5 install package**
`Install-Package AWSSDK.Core`
`Install-Package AWSSDK.DynamoDBv2`
`Install-Package AWSSDK.Extensions.NETCore.Setup`

**6 Add aws services before build();**
```bash
var awsOptions = builder.Configuration.GetAWSOptions();
builder.Services.AddDefaultAWSOptions(awsOptions);
builder.Services.AddAWSService<IAmazonDynamoDB>();
builder.Services.AddScoped<IDynamoDBContext, DynamoDBContext>();
```

**7 Add StudentsController from this repo from Controllers folder**

**8 Run application it will open API swagger run executes all endpoints for CRUD operation **



## S3 Bucket in API

**1 Attach policy to studentAPI user**
`AmazonS3FullAccess`

**2 Add NuGet packages in your project**
`Install-Package AWSSDK.S3`
`Install-Package AWSSDK.Extensions.NETCore.Setup`

**3 Add below lines in Startup class**
```bash
builder.Services.AddDefaultAWSOptions(builder.Configuration.GetAWSOptions());
builder.Services.AddAWSService<IAmazonS3>();
```

**4 Add BucketsController from this repo from Controllers folder**
Here you can find CRUD operation for S3 buckets


**5 Add FilesController from this repo from Controllers folder**
Here you can find Upload, Get and delete file operations in S3 buckets


## Serverless API in Lambda 

**1 Install aws package in existing Asp.net Core API project**
Install-Package Amazon.Lambda.AspNetCoreServer.Hosting

**2 Add below line in Startup class**
`builder.Services.AddAWSLambdaHosting(LambdaEventSource.HttpApi);`

**3 To deploy/redeploy lambda**
Run below command
`dotnet lambda deploy-function`

- Enter runtime

dotnet6

- Enter Function Name:

studentApi

- Select IAM Role:

Select **Create new Role**

- Enter name of Iam Role:

Select IAM Policy to attach new Role:

Select **4 AwsLambdaBasicExecutionRole**

- Enter Memory Size:

256

- Enter Timeout:

30

- Enter Handler 

**make sure namespace of your project**

Check your lambda in AWS Console

**4 Attach policies in console**
```bash
AmazonS3FullAccess
AmazonDynamoDBFullAccess
AmazonDynamoDBReadOnlyAccess
AWSLambdaDynamoDBExecutionRole
AWSLambdaInvocation-DynamoDB
```

**4 Add Function URL in Lambda**
Go to Lambda -> Configuration -> Function URL

- Click on Create function URL
- Select Auth type: NONE
- Click Save

**5 Navigate to function URL and use it from postman to call Student, buckets and files api**
