# aws-04-delete-item

---

## AWS Lambda

1. Log on to `AWS Lambda console`
2. `Create function - Author from scratch` provide a name, choose runtime `Node.js 16.x`, architecture `x86_64` then click on `Create function`

JS code (paste in index.js) of the function to delete item:

```js
const AWS = require("aws-sdk");
// Instantiate a DynamoDB document client with the SDK
let dynamodb = new AWS.DynamoDB.DocumentClient();
// Define handler function, the entry point to our code for the Lambda service
// We receive the object that triggers the function as a parameter
exports.handler = async (event) => {
  // Extract values from event and format as strings
  let values = JSON.stringify(`id: ${event.id}`);
  // Create JSON object with parameters for DynamoDB and store in a variable
  let params = {
    TableName: "YOUR-TABLE-NAME",
    Key: {
      id: event.id,
    },
  };
  // Using await, make sure object writes to DynamoDB table before continuing execution
  await dynamodb.delete(params).promise();
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

DynamoDB, browse to your table and copy an item id to test delete.

Lambda, click on `Test` button to configure a test event, provide a name and paste below json with above copied id in it:

```json
{
  "id": "<DynamoDB item id>"
}
```

Click on `Test` orange button after having saved the test and as a result you may get a 200 result code.

Observe in your DynamoDB table that item has correctly been deleted.

---

## Amazon API Gateway

### Select REST API

1. Log on to `Amazon API Gateway`
2. Select API created in [aws-02-create-item](aws-02-create-item.md)

### Create new resource and method

1. In the left pane, click Resources under your API.
2. Select the “/” resource, then click Create Method from the Action drop-down menu.
3. Select `DELETE` from the drop-down menu that appears, then click the check mark.
4. Select Lambda Function as the integration type.
5. Enter Function Name in the Function field.
6. Click the blue Save button.
7. You should see a message telling you that you allow the API being created to call your Lambda function. Click the OK button.
8. Select the newly created `DELETE` method, then Enable CORS mechanism from the Action drop-down menu.
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
2. The methods available for the API are displayed on the right. Click `DELETE`.
3. Click the blue lightning bolt icon.
4. In `Request Body` field paste an id in a JSON the same way you did it above to [test function](#test-function).
5. Click the blue Test button.
6. On the right, a response with Code 200 should appear.
7. Observe in your DynamoDB table that item has correctly been deleted.

---

## Web app delete item

Browse to `API Gateway` click on API you choose to use then select `Stages` in left column, expand Stages and select `DELETE` method created previously above.

Copy `Invoke URL`.

Paste it in `index.js` file of the blog project, as follow:

```js
..
const createArticles = (articles) => {
  const articlesDOM = articles.map((article) => {
    const articleDOM = document.createElement("div");
    articleDOM.classList.add("article");
    articleDOM.innerHTML = `
<img
  src="${article.img}"
  alt="profile"
/>
<h2>${article.title}</h2>
<p class="article-author">${article.author} - ${article.category}</p>
<p class="article-content">
  ${article.content}
</p>
<div class="article-actions">
  <button class="btn btn-delete" data-id=${article.id} >Delete</button>
</div>
`;
    return articleDOM;
  });
  articleContainerElement.innerHTML = "";
  articleContainerElement.append(...articlesDOM);
  const deleteButtons = articleContainerElement.querySelectorAll(".btn-delete");
  console.log(deleteButtons);
  deleteButtons.forEach((button) => {
    button.addEventListener("click", async (event) => {
      try {
        const target = event.target;
        const articleId = target.dataset.id;
        const articleToDelete = new Object();
        articleToDelete.id = articleId;
        const json = JSON.stringify(articleToDelete);
        console.log(json);
        const response = await fetch(
          `<Invoke URL>`,
          {
            method: "DELETE",
            body: json,
            headers: {
              "Content-Type": "application/json",
            },
          }
        );
        const body = await response.json();
        console.log(body);
        fetchArticle();
      } catch (e) {
        console.log("e: ", e);
      }
    });
  });
};
..
```

### Test it locally

```console
npm start
```

Browse to [localhost:4000](localhost:4000)

Delete an article and observe in main page (index) that item has correctly been deleted.

### Test it remotely

Push code and observe automatic deployment when you browse to `AWS Amplify` and click on your application.

Browse to [aws hosted application](https://aws-to-doc.d2nxetbv9qp6jv.amplifyapp.com/)

Delete an article and observe in main page (index) that item has correctly been deleted.

---
