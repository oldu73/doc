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

JS code (paste in index.js) of the function to create item:

```js
const AWS = require("aws-sdk");
// Instantiate a DynamoDB document client with the SDK
let dynamodb = new AWS.DynamoDB.DocumentClient();
// Define handler function, the entry point to our code for the Lambda service
// We receive the object that triggers the function as a parameter
exports.handler = async (event, context) => {
  let id = context.awsRequestId;
  // Extract values from event and format as strings
  let values = JSON.stringify(
    `id: ${id}, author: ${event.author}, img: ${event.img}, category: ${event.category}, title: ${event.title}, content: ${event.content}`
  );
  // Create JSON object with parameters for DynamoDB and store in a variable
  let params = {
    TableName: "YOUR-TABLE-NAME",
    Item: {
      id: id,
      author: event.author,
      img: event.img,
      category: event.category,
      title: event.title,
      content: event.content,
    },
  };
  // Using await, make sure object writes to DynamoDB table before continuing execution
  await dynamodb.put(params).promise();
  // Create a JSON object with our response and store it in a constant
  const response = {
    statusCode: 200,
    body: values,
  };
  // Return the response constant
  return response;
};
```

Then click on deploy.

### Add permission

Using AWS Identity and Access Management (IAM), add permissions to your function so that it can use the DynamoDB service.

Copy DynamoDB table `ARN`.

Steps:

1. Click the Configuration - Permissions tab.
2. In the Execution Role field, click the appropriate role. A new tab opens in your browser.
3. Click Add inline policy to the right of Authorization policies and select the JSON tab.
4. Copy below json into the text box, being careful to replace your table's `ARN` in the Resource field, line 15:
5. This permission allows your Lambda function to read, modify, or delete items, but only in the table you created.
6. Click the Review Policy button, colored blue.
7. Next to Name, type policy name.
8. Click the Create Policy button, colored blue.
9. You can now close this tab and return to the one dedicated to your Lambda function.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem",
        "dynamodb:DeleteItem",
        "dynamodb:GetItem",
        "dynamodb:Scan",
        "dynamodb:Query",
        "dynamodb:UpdateItem"
      ],
      "Resource": "YOUR-TABLE-ARN"
    }
  ]
}
```

### Test function

Click on `Test` button to configure a test event, provide a name and paste below json:

```json
{
  "author": "Author 48",
  "img": "https://randomuser.me/api/portraits/men/48.jpg",
  "category": "Category 48",
  "title": "Title 48",
  "content": "Hello 48 Hello 48 Hello 48 Hello 48"
}
```

To validate test is successful, browse to database to see above element has correctly been created.

---

## Amazon API Gateway

1. Log on to `Amazon API Gateway`
2. Create API - REST API - Build
3. Choose `REST` protocol
4. Create `New API`
5. Settings, provide a name, choose Endpoint Type as `Edge optimized` it is the most suitable option for public services accessed from the Internet

...

---
