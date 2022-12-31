# aws-07-sort-api

Two new resources (API) with Lambda integration to sort articles by creation dates ascending and descending.

To notice that a `timestamp` (epoch type Number) has been added to `DynamoDB` table to facilitate the filter function.

---

## Lambda

### create item

Create item with timestamp, function-createItem:

```js
const AWS = require("aws-sdk");
// Instantiate a DynamoDB document client with the SDK
let dynamodb = new AWS.DynamoDB.DocumentClient();
// Define handler function, the entry point to our code for the Lambda service
// We receive the object that triggers the function as a parameter
exports.handler = async (event, context) => {
  // id from context as unique key to record in database
  let id = context.awsRequestId;
  let createdAt = new Date().toISOString();
  let timeStamp = Date.now();
  // Extract values from event and format as strings
  let values = JSON.stringify(
    `id: ${id}, createdAt: ${createdAt}, timestamp: ${timeStamp}, author: ${event.author}, img: ${event.img}, category: ${event.category}, title: ${event.title}, content: ${event.content}`
  );
  // Create JSON object with parameters for DynamoDB and store in a variable
  let params = {
    TableName: "<Table Name>",
    Item: {
      id: id,
      createdAt: createdAt,
      timestamp: timeStamp,
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

### list ascending

List articles in ascending order, function-listItems-asc:

```js
const AWS = require("aws-sdk");
const docClient = new AWS.DynamoDB.DocumentClient();

const params = {
  TableName: "<Table Name>",
};

async function listItems() {
  try {
    const data = await docClient.scan(params).promise();
    //return data;  // no sort
    return data.Items.sort((a, b) => a.timestamp - b.timestamp); // oldest (asc) date on top
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

### list descending

List articles in descending order, function-listItems-desc:

```js
const AWS = require("aws-sdk");
const docClient = new AWS.DynamoDB.DocumentClient();

const params = {
  TableName: "<Table Name>",
};

async function listItems() {
  try {
    const data = await docClient.scan(params).promise();
    //return data;  // no sort
    return data.Items.sort((a, b) => b.timestamp - a.timestamp); // youngest (desc) date on top
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

---

## API Gateway

Add two new resources with Lambda integration for `GET` methods:

- articles/asc
- articles/desc

Point to new Lambdas created above.

Be aware of `Resource Policy` for allowed Ip(s) to use the API.

Enable CORS for two new methods.

Then deploy API.

---
