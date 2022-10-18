# aws-hosting

How to host the project on `AWS Cloud`.

The objective is to host the blog project on `AWS Cloud` with `AWS Amplify` and to create the API with `Amazon API Gateway` to interact with `Amazon DynamoDB` through `AWS Lambda` controlled by `AWS Identity and Access Management`.

This documentation is based on Amazon tutorial for [Creating a Simple Web Application](https://aws.amazon.com/fr/getting-started/hands-on/build-web-app-s3-lambda-api-gateway-dynamodb/).

All the time, pay attention in which geographical region the different components of your application are located.

---

## AWS Amplify

Create web application with `AWS Amplify`.

1. Log on to [AWS Management Console](https://aws.amazon.com/fr/console/)  
2. Browse to `AWS Amplify console`  
3. Click on `New app` - `Host web app`  
4. Select `GitHub`  
5. Select repository and branch  
6. Build and test settings are auto-detected, click `Next` then `Save and deploy`  
7. Protect your app with a password in `App settings - Access control - Manage access`, `Access setting = Restricted - password required`, then fill `Username and Password`

---

## Amazon DynamoDB

1. Log on to `Amazon DynamoDB console`  
2. Click on `Create table`, provide a `Table name`  
3. Choose `id` as `Partition key` name of `String` type  
4. Let all the rest as default and click on `Create table`  
5. Be aware of Amazon Resource Name (ARN) of the table, we will need it later

---

## AWS Lambda

1. Log on to `AWS Lambda console`  
2. `Create function - Author from scratch` provide a name, choose runtime `Node.js 16.x`, architecture `x86_64` then click on `Create function`  

### Create item

JS code of the function to create item  
...

Using AWS Identity and Access Management (IAM), add permissions to your function so that it can use the DynamoDB service  
...

Test function  
...

---
