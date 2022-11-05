# aws-03-list-item

---

## AWS Lambda

1. Log on to `AWS Lambda console`
2. `Create function - Author from scratch` provide a name, choose runtime `Node.js 16.x`, architecture `x86_64` then click on `Create function`

JS code (paste in index.js) of the function to list item:

```js
const AWS = require("aws-sdk");
const docClient = new AWS.DynamoDB.DocumentClient();

const params = {
  TableName: "YOUR-TABLE-NAME",
};

async function listItems() {
  try {
    const data = await docClient.scan(params).promise();
    return data;
  } catch (err) {
    return err;
  }
}

exports.handler = async (event, context) => {
  try {
    const data = await listItems();
    return { body: data };
    //    return { body: JSON.stringify(data) }
  } catch (err) {
    return { error: err };
  }
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

### Test function

Click on `Test` button to configure a test event, provide a name and paste below json:

```json
{}
```

Click on `Test` orange button after having saved the test and as a result you should get database content.

---

## Amazon API Gateway

### Select REST API

1. Log on to `Amazon API Gateway`
2. Select API created in [aws-02-create-item](aws-02-create-item.md)

### Create new resource and method

1. In the left pane, click Resources under your API.
2. Select the “/” resource, then click Create Method from the Action drop-down menu.
3. Select `GET` from the drop-down menu that appears, then click the check mark.
4. Select Lambda Function as the integration type.
5. Enter Function Name in the Function field.
6. Click the blue Save button.
7. You should see a message telling you that you allow the API being created to call your Lambda function. Click the OK button.
8. Select the newly created `GET` method, then Enable CORS mechanism from the Action drop-down menu.
9. A message asking you to confirm the changes made to the method should appear. Click the blue Yes, Replace Existing Values button.
10. [To allow only specific IP addresses to access your API Gateway REST API](https://aws.amazon.com/fr/premiumsupport/knowledge-center/api-gateway-resource-policy-access/)

### API deployment

1. From the Actions drop-down list, select Deploy API.
2. Select existing `dev` stage created in [aws-02-create-item](aws-02-create-item.md) from the Deploy Stage drop-down list.
3. Select Deploy.
4. Click on blue Save button.
5. Copy the URL next to Invoke URL (you will need this later) and save.

### API validation

1. On the left, click Resources.
2. The methods available for the API are displayed on the right. Click `GET`.
3. Click the blue lightning bolt icon.
4. Click the blue Test button.
5. On the right, a response with Code 200 should appear and Response Body field with data content from db through lambda.

---

## Web app list item

Browse to `API Gateway` click on API you choose to use then select `Stages` in left column, expand Stages and select `GET` method created previously above.

Copy `Invoke URL`.

Paste it in `index.js` file of the blog project, as follow:

```js
..
const fetchArticle = async () => {
  try {
    const response = await fetch(
      `<Invoke URL>`,
      {
        method: "GET",
      }
    );
    let content = await response.json();
    let articles = content.body.Items;
    console.log(articles);
    // Standard api behavior return not an array if only one element
    // so below code convert it (one element) into an array
    // otherwise it cause an error when 'map' method (to create articles) will be called on it.
    if (!Array.isArray(articles)) {
      articles = [articles];
    }
    createArticles(articles);
  } catch (e) {
    console.log("e : ", e);
  }
};
..
```

### Test it locally

```console
npm start
```

Browse to [localhost:4000](localhost:4000)

Create an article, save and observe in main page (index) newly created article and rest of the list as well.

### Test it remotely

Push code and observe automatic deployment when you browse to `AWS Amplify` and click on your application.

Browse to [aws hosted application](https://aws-to-doc.d2nxetbv9qp6jv.amplifyapp.com/)

Create an article, save and observe in main page (index) newly created article and rest of the list as well.

---
