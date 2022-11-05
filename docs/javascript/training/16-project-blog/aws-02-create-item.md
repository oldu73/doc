# aws-02-create-item

---

## AWS Lambda

1. Log on to `AWS Lambda console`
2. `Create function - Author from scratch` provide a name, choose runtime `Node.js 16.x`, architecture `x86_64` then click on `Create function`

JS code (paste in index.js) of the function to create item:

```js
const AWS = require("aws-sdk");
// Instantiate a DynamoDB document client with the SDK
let dynamodb = new AWS.DynamoDB.DocumentClient();
// Define handler function, the entry point to our code for the Lambda service
// We receive the object that triggers the function as a parameter
exports.handler = async (event, context) => {
  // id from context as unique key to record in database
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

Notice that the id is taken from the context (`let id = context.awsRequestId`) as the unique key to record in database.

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

### Create new REST API

1. Log on to `Amazon API Gateway`
2. Create API - REST API - Build
3. Choose `REST` protocol
4. Create `New API`
5. Settings, provide a name, choose Endpoint Type as `Edge optimized` it is the most suitable option for public services accessed from the Internet

### Create new resource and method

1. In the left pane, click Resources under your API.
2. Select the “/” resource, then click Create Method from the Action drop-down menu.
3. Select `POST` from the drop-down menu that appears, then click the check mark.
4. Select Lambda Function as the integration type.
5. Enter Function Name in the Function field.
6. Click the blue Save button.
7. You should see a message telling you that you allow the API being created to call your Lambda function. Click the OK button.
8. Select the newly created `POST` method, then Enable CORS mechanism from the Action drop-down menu.
9. Leave the `POST` box unchecked and click the blue Enable CORS mechanism and replace existing CORS headers button.
10. A message asking you to confirm the changes made to the method should appear. Click the blue Yes, Replace Existing Values button.
11. [To allow only specific IP addresses to access your API Gateway REST API](https://aws.amazon.com/fr/premiumsupport/knowledge-center/api-gateway-resource-policy-access/)

### API deployment

1. From the Actions drop-down list, select Deploy API.
2. Select [New Stage] from the Deploy Stage drop-down list.
3. Enter dev in the Stage Name field.
4. Select Deploy.
5. Copy and save the URL next to Invoke URL (you will need this later).

### API validation

1. On the left, click Resources.
2. The methods available for the API are displayed on the right. Click `POST`.
3. Click the blue lightning bolt icon.
4. Paste a JSON that correspond to data format you want to store in your database, into the Request Body field.
5. Click the blue Test button.
6. On the right, a response with Code 200 should appear.
7. Congratulations! You have created and tested an API for calling a Lambda function.

---

## Web app create item

Browse to `API Gateway` click on API you choose to use then select `Stages` in left column, expand Stages and select `POST` method created previously above.

Copy `Invoke URL`.

Paste it in `form.js` file of the blog project, as follow:

```js
..
form.addEventListener("submit", async (event) => {
  ..
  try {
    if (formIsValid(article)) {
      const json = JSON.stringify(article);
      const response = await fetch(
        "<Invoke URL>",
        {
          method: "POST",
          body: json,
          headers: {
            "Content-Type": "application/json",
          },
        }
      );
      const body = await response.json();
      console.log(body);
    }
  } catch (e) {
    console.error("e: ", e);
  }
});
..
```

### Test it locally

```console
npm start
```

Browse to [localhost:4000](localhost:4000)

Create an article, save and observe in DynamoDB that it has been created.

### Test it remotely

Push code and observe automatic deployment when you browse to `AWS Amplify` and click on your application.

Browse to [aws hosted application](https://aws-to-doc.d2nxetbv9qp6jv.amplifyapp.com/)

Create an article, save and observe in DynamoDB that it has been created.

---
