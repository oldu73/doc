# aws-05-get-item

---

## AWS Lambda

1. Log on to `AWS Lambda console`
2. `Create function - Author from scratch` provide a name, choose runtime `Node.js 16.x`, architecture `x86_64` then click on `Create function`

JS code (paste in index.js) of the function to get item:

```js
const AWS = require("aws-sdk");
// Instantiate a DynamoDB document client with the SDK
let dynamodb = new AWS.DynamoDB.DocumentClient();
// Define handler function, the entry point to our code for the Lambda service
// We receive the object that triggers the function as a parameter
exports.handler = async (event) => {
  // Create JSON object with parameters for DynamoDB and store in a variable
  let params = {
    TableName: "YOUR-TABLE-NAME",
    Key: {
      id: event.queryStringParameters.id,
    },
  };
  // Using await, make sure object reads from DynamoDB table before continuing execution
  const data = await dynamodb.get(params).promise();
  // Create a JSON object with our response and store it in a constant
  const response = {
    statusCode: 200,
    headers: {
      "Access-Control-Allow-Headers": "Content-Type",
      "Access-Control-Allow-Origin":
        "https://<amplify app url> or <localhost:4000>",
      "Access-Control-Allow-Methods": "OPTIONS,POST,GET",
    },
    body: JSON.stringify(data),
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
6. Click the Review Policy blue button.
7. Next to Name, type policy name.
8. Click the Create Policy blue button.
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

## Amazon API Gateway

### Select REST API

1. Log on to `Amazon API Gateway`
2. Select API created in [aws-02-create-item](../16-project-blog/aws-02-create-item.md)

### Create new resource and method

1. In the left pane, click Resources under your API.
2. Select the “/” resource, then click Create Resource from the Action drop-down menu, to create a new resource called `article`.
3. Select the “/article” resource, then click Create Method from the Action drop-down menu.
4. Select `GET` from the drop-down menu that appears, then click the check mark.
5. Select Lambda Function as the integration type.
6. Check `Use Lambda Proxy integration` to allow query string parameter from header in lambda (article id).
7. Enter Function Name in the Function field.
8. Click the blue Save button.
9. You should see a message telling you that you allow the API being created to call your Lambda function. Click the OK button.
10. Select the newly created `GET` method, then Enable CORS mechanism from the Action drop-down menu.
11. A message asking you to confirm the changes made to the method should appear. Click the blue Yes, Replace Existing Values button.
12. [To allow only specific IP addresses to access your API Gateway REST API](https://aws.amazon.com/fr/premiumsupport/knowledge-center/api-gateway-resource-policy-access/)
13. Edit Method Request to add `id` in `URL Query String Parameters` section as `Required`, you may also check `Request Validator` in header section of this form.

### API deployment

1. From the Actions drop-down list, select Deploy API.
2. Select existing `dev` stage created in [aws-02-create-item](../16-project-blog/aws-02-create-item.md) from the Deploy Stage drop-down list.
3. Select Deploy.
4. Click on blue Save button.
5. Copy the URL next to Invoke URL (you will need this later) and save.

---

## Web app edit item

This part (get item) is the first of two parts (get + update) in the edit process.

Application to edit, described in [138-edit-article](138-edit-article.md) is aws ready.

---
