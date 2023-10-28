# pre sign-up

react - sign up - create user in DynamoDB in addition of Cognito - dynapp

Automatically Create a Dynamo DB Record from a Cognito User Pool | Amazon Web Services

User record, with name, Cognito UUID and UID of 6 characters (nano ID).

[SRC](https://www.youtube.com/watch?v=Ti0Nc_FHZLo)

[github project repo](https://github.com/oldu73/dynapp)

---

## New app

Login to [aws console](https://aws.amazon.com/fr/console/) browse to `AWS Amplify` and click on `New app` button, select `Build an app`.

Give the app a name, e.g. "dynapp" and click on `Confirm deployement`.

Once it's done click on `Launch Studio` button.

---

## Authentication

In Amplify Studio, click on `Authentication` menu in the side bar.

Leave `email` as login mechanisms.

Add `Name` attribute.

Uncheck everything in `Password protection settings` and length of 6 characters, to make password weak for testing.

Don't forget to provide strong protection for the password once in production.

Click on `Deploy` button then `Confirm deployement`

---

## Database

Click on `Data` in the side bar menu, then `Add model` to add a new table, name the model `User`.

Add `foreignId` required ID field,
Add `name` required String field,
Add `uid` required String field,

In GraphQL schema view you should have:

```GraphQL
type User @model @auth(rules: [{allow: public}]) {
  id: ID!
  foreignId: ID!
  name: String!
  uid: String!
}
```

Then click on `Save and deploy`.

---

## Connect backend to frontend

Create a react app, in a command prompt, type:

```console
npx create-react-app dynapp
cd dynapp
npm install aws-amplify @aws-amplify/ui-react
npm start
```

In Amplify Studio, click on top right link `Deployment successful - click for next steps` and copy command to get the latest configuration files.

Amplify CLI should be installed on the machine.

Then run above copied command in a command prompt at the root level of your react project, e.g.:

```console
amplify pull --appId <id> --envName staging
```

Confirm in the browser that you want to login to the Amplify CLI.

Choose `IntelliJ IDEA` for `WebStorm` and all the rest to default.

Once the pull command is done, launch WebStorm and open the `dynapp` project.

### index.js

Edit `index.js` file in `src` folder to add import for amplify:

```js
import ..

import { Amplify } from "aws-amplify";
import awsconfig from "./aws-exports";
Amplify.configure(awsconfig);

const root ..
```

### App.js

Edit `App.js` file in `src` folder to add authentication to the application:

```js
import ..

import "@aws-amplify/ui-react/styles.css";
import { withAuthenticator } from "@aws-amplify/ui-react";

function App() {
  ..
}

export default withAuthenticator(App);
```

---

## Lambda trigger

### Create function

In command prompt, at the root level of the project:

```console
amplify update auth
> Walkthrough all the auth configurations
.. select all default except OAuth to No
? Do you want to configure Lambda Triggers for Cognito? Yes
>(*) Pre Sign-up
>(*) Create your own module
```

### Give function access to GraphQL API

In command prompt, at the root level of the project:

```console
amplify update function
? Select the Lambda function you want to update (Use arrow keys)
> dynappPreSignup
? Which setting do you want to update? (Use arrow keys)
> Resource access permissions
? Select the categories you want this function to have access to.
>(*) api
? Select the operations you want to permit on dynapp2
>(*) Mutation
You can access the following resource attributes as environment variables from your Lambda function
        API_DYNAPP_GRAPHQLAPIENDPOINTOUTPUT
        API_DYNAPP_GRAPHQLAPIIDOUTPUT
        API_DYNAPP_GRAPHQLAPIKEYOUTPUT
```

### Install node-fetch

In command prompt, at the root level of the project:

```console
cd amplify\backend\function\dynappPreSignup\src
npm i node-fetch@2
```

### Install nano id

In package.json of the pre signup lambda add nano id dependency:

```json
{
  ..,
  "dependencies": {
    ..,
    "nanoid": "^5.0.1"
  }
}

```

Then run `npm install` in lambda folder.

### Custom trigger, custom.js

In your IDE open `amplify/backend/function/dynappPreSignup/src/custom.js`, remove everything and bring in it code we need for lambda function as follow:

```js
/**
 * @type {import('@types/aws-lambda').APIGatewayProxyHandler}
 */

const fetch = require("node-fetch");

exports.handler = async (event, context) => {
  const GRAPHQL_ENDPOINT = process.env.API_DYNAPP_GRAPHQLAPIENDPOINTOUTPUT;
  const GRAPHQL_API_KEY = process.env.API_DYNAPP_GRAPHQLAPIKEYOUTPUT;

  // Retrieve logs in Lambda - Monitor - Logs - CloudWatch
  // console.log("Hello, world!");
  // console.log(JSON.stringify(event, null, 2));

  const { customAlphabet } = await import("nanoid");

  const nanoid = customAlphabet('0123456789abcdefghijklmnopqrstuvwxyz', 6)
  let uid = nanoid();

  const query = /* GraphQL */ `
    mutation CREATE_USER($input: CreateUserInput!) {
      createUser(input: $input) {
        name,
        foreignId,
        uid,
      }
    }
  `;

  const variables = {
    input: {
      name: event.request.userAttributes.name,
      foreignId: event.userName,  // event.userName contain Cognito UUID of the user
      uid: uid,
    },
  };

  const options = {
    method: "POST",
    headers: {
      "x-api-key": GRAPHQL_API_KEY,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({query, variables}),
  }

  const response = {};

  try {
    const res = await fetch(GRAPHQL_ENDPOINT, options);
    response.data = await res.json();
    if (response.data.errors) response.statusCode = 400;
  } catch (error) {
    response.statusCode = 400;
    response.body = {
      errors: [
        {
          message: error.message,
          stack: error.stack,
        }
      ]
    }
  }

  return {
    ...response,
    body: JSON.stringify(response.body),
  };
};
```

In command prompt update remote:

```
amplify push
```

---

## Next steps

- \<Authenticator\> .. \</Authenticator\> [SRC](https://stackoverflow.com/questions/70036160/amplifysignout-is-not-exported-from-aws-amplify-ui-react)  
- \<AmplifySignOut /\> button [SRC](https://www.youtube.com/watch?v=Sk9HMuAaTmQ)  
- post confirmation mail with "uid" [SRC](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-post-confirmation.html)

---
