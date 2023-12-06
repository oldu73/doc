# share<###>dyn

Share Dyn is an application for sharing todos in real time between users.

In addition to the basic tutorial, we prepare the application with the User Detail table to be able to share with full user names, short name or short unique identifier (nano ID) rather than Cognito user UUIDs for the interface with the user, even if in the end it is indeed the Cognito UUID which will prevail.

Current version [share003dyn](https://github.com/oldu73/share003dyn)

---

app mix of [sharetodo](https://github.com/oldu73/share-todo) + [dynapp](https://github.com/oldu73/dynapp) = sharedyn

From sources:

- [Automatically Create a Dynamo DB Record from a Cognito User Pool | Amazon Web Services](https://www.youtube.com/watch?v=Ti0Nc_FHZLo)  
- [Build real-time multi-user experiences using GraphQL on AWS Amplify](https://aws.amazon.com/fr/blogs/mobile/build-real-time-multi-user-experiences-using-graphql-on-aws-amplify/)

[aws amplify react tutorial](https://docs.amplify.aws/start/q/integration/react/)

Remark: - Amplify choices in console, when not specified = default choice e.g. y/N, hit enter means "No".

---

## create react app

```console
npx create-react-app share<###>dyn
cd share<###>dyn
```

## amplify init

```console
amplify init
? Initialize the project with the above configuration? No
..
? Choose your default editor: IntelliJ IDEA
..
? Select the authentication method you want to use: AWS profile	// for aws region
? Please choose the profile you want to use amplify-dev // region = N. Virginia (cheapest)
```

Check in aws console web app that project has correctly been set up in the right region.

Click on `Set up Amplify Studio`.

## amplify add api

```console
amplify add api
? Select from one of the below mentioned services: GraphQL
? Here is the GraphQL API that we will create. Select a setting to edit or continue Authorization modes: API key (default, expiration tim
e: 7 days from now)
? Choose the default authorization type for the API Amazon Cognito User Pool
Using service: Cognito, provided by: awscloudformation

 The current configured provider is Amazon Cognito.

 Do you want to use the default authentication and security configuration? Manual configuration
 Select the authentication/authorization services that you want to use: User Sign-Up, Sign-In, connected with AWS IAM controls (Enables p
er-user Storage features for images or other content, Analytics, and more)
 Provide a friendly name for your resource that will be used to label this category in the project: share001dynf027ea0ff027ea0f
 Enter a name for your identity pool. share001dynf027ea0f_identitypool_f027ea0f
 Allow unauthenticated logins? (Provides scoped down permissions that you can control via AWS IAM) No
 Do you want to enable 3rd party authentication providers in your identity pool? No
 Provide a name for your user pool: share001dynf027ea0f_userpool_f027ea0f
 Warning: you will not be able to edit these selections.
 How do you want users to be able to sign in? Email
 Do you want to add User Pool Groups? Yes
? Provide a name for your user pool group: authenticated
? Do you want to add another User Pool Group No
√ Sort the user pool groups in order of preference · authenticated
 Do you want to add an admin queries API? No
 Multifactor authentication (MFA) user login options: OFF
 Email based user registration/forgot password: Enabled (Requires per-user email entry at registration)
 Specify an email verification subject: Your verification code
 Specify an email verification message: Your verification code is {####}
 Do you want to override the default password policy for this User Pool? Yes
 Enter the minimum password length for this User Pool: 8
 Select the password character requirements for your userpool:
 Warning: you will not be able to edit these selections.
 What attributes are required for signing up? Email, Name
 Specify the app's refresh token expiration period (in days): 30
 Do you want to specify the user attributes this app can read and write? No
 Do you want to enable any of the following capabilities?
 Do you want to use an OAuth flow? No
? Do you want to configure Lambda Triggers for Cognito? Yes
? Which triggers do you want to enable for Cognito Post Confirmation
? What functionality do you want to use for Post Confirmation Add User To Group, Create your own module
√ Enter the name of the group to which users will be added. · authenticated
✅ Successfully added resource share002dyndd303979dd303979PostConfirmation locally.

✅ Next steps:
Check out sample function code generated in <project-dir>/amplify/backend/function/share002dyndd303979dd303979PostConfirmation/src
"amplify function build" builds all of your functions currently in the project
"amplify mock function <functionName>" runs your function locally
To access AWS resources outside of this Amplify app, edit the C:\git\aws\share002dyn\amplify\backend\function\share002dyndd303979dd303979PostConfirmation\custom-policies.json
"amplify push" builds all of your local backend resources and provisions them in the cloud
"amplify publish" builds all of your local backend and front-end resources (if you added hosting category) and provisions them in the cloud
Successfully added the Lambda function locally
? Do you want to edit your add-to-group function now? No
? Do you want to edit your custom function now? No
✅ Successfully added auth resource share002dyndd303979dd303979 locally

✅ Some next steps:
"amplify push" will build all your local backend resources and provision it in the cloud
"amplify publish" will build all your local backend and frontend resources (if you have hosting category added) and provision it in the cloud

? Configure additional auth types? No
? Here is the GraphQL API that we will create. Select a setting to edit or continue Continue
? Choose a schema template: Single object with fields (e.g., “Todo” with ID, name, description)

⚠️  WARNING: Some types do not have authorization rules configured. That means all create, read, update, and delete operations are denied on these types:
         - Todo
Learn more about "@auth" authorization rules here: https://docs.amplify.aws/cli/graphql/authorization-rules
✅ GraphQL schema compiled successfully.

Edit your schema at C:\git\aws\share002dyn\amplify\backend\api\share002dyn\schema.graphql or place .graphql files in a directory at C:\git\aws\share002dyn\amplify\backend\api\share002dyn\schema
√ Do you want to edit the schema now? (Y/n) · no
✅ Successfully added resource share002dyn locally

✅ Some next steps:
"amplify push" will build all your local backend resources and provision it in the cloud
"amplify publish" will build all your local backend and frontend resources (if you have hosting category added) and provision it in the cloud
```

!! Warning!! With this setup, password is only 8 characters with no special restrictions to enhance security, !don't let it like this in production environment!

## graphql

Open project in IDE and edit graphql schema in file `amplify/backend/api/sharedyn/schema.graphql` as follow:

```graphql
type Share<###>DynTo<###>Do
@model # Creates a DynamoDB table
@auth(rules: [{ allow: owner, ownerField: "owners"}]) # Sets up owner-based authorization
{
  id: ID!
  content: String
  owners: [String] # Use a String array type to configure multi-owner authorization
}

type Share<###>DynUser<###>Detail
@model
@auth(rules: [{ allow: groups, groups: ["authenticated"] }])
{
  id: ID!
  name: String!
  shortName: String!
  uid: String!
}
```

```console
cd function share<###>dyn<id>PostConfirmation 'src' folder and run 'npm install'
cd project's root folder
amplify push
(.. all default answers)
? Do you want to generate code for your newly created GraphQL API Yes
? Choose the code generation language target javascript
? Enter the file name pattern of graphql queries, mutations and subscriptions src\graphql\**\*.js
? Do you want to generate/update all possible GraphQL operations - queries, mutations and subscriptions Yes
? Enter maximum statement depth [increase from default if your schema is deeply nested] 2
```

Remark: - If something has change in the cloud, then, on local, pull latest is needed, like for e.g.:

```console
amplify pull --appId <id> --envName dev
```

Check in Amplify Studio - Data, that everything has been correctly deployed in the cloud.

## npm install amplify

In a command prompt (project's root folder), install npm amplify packages:

- [aws-amplify](https://www.npmjs.com/package/aws-amplify) AWS Amplify is a JavaScript library for frontend and mobile developers building cloud-enabled applications.
- [@aws-amplify/ui-react](https://www.npmjs.com/package/@aws-amplify/ui-react) Amplify UI is an open-source UI library with cloud-connected components that are endlessly customizable, accessible, and can integrate into any application.

```console
npm install aws-amplify @aws-amplify/ui-react
npm start
```

## index.js to import amplify

Edit `index.js` file in `src` folder to add import for amplify:

```js
import ..
..
import { Amplify } from "aws-amplify";
import awsconfig from "./aws-exports";

Amplify.configure(awsconfig);
..
const root ..
```

## App.js to add authentication (and display user login information and signOut action)

Edit `App.js` file in `src` folder to add authentication to the application, to show who’s logged and give user the ability to log out.

Replace the contents of App.js with the following code:

```js
import './App.css';

import {Amplify} from "aws-amplify";
import awsconfig from "./aws-exports";

import "@aws-amplify/ui-react/styles.css";
import {withAuthenticator, Text, Flex, View, Badge, Button} from '@aws-amplify/ui-react';

Amplify.configure(awsconfig);

function App({user, signOut}) {
  // console.log(user);
  return (
    <Flex direction={"column"} padding={8}>
      <Text>Logged in as <b>{user.signInDetails.loginId}</b> <Button variation='link' onClick={signOut}>Sign out</Button></Text>
    </Flex>
  );
}

export default withAuthenticator(App);
```

Test sign-up, in, out and then delete user

## amplify update PreSignup function to add user in DynamoDB, table Share<###>DynUser<###>Detail

Retrieve your Lambda (e.g. from Cognito) for PostConfirmation, share<###>dyn<id\>PostConfirmation-<stage\>

Click on Configuration tab, then Permissions and click on role.

In role, click on Add permissions, select Create inline policy.

Choose service as DynamoDB.

Check Read, GetItem and Write, PutItem

Resources, Specific click on ARN and paste the one from your DynamoDB table Share<###>DynUser<###>Detail-<id\>-<stage\>

Click next and give policy a name like Dynamo<###>Policy and then click on Create policy.

In your IDE edit custom.js of your PostConfirmation lambda function with code below:

```js
/**
 * @type {import('@types/aws-lambda').APIGatewayProxyHandler}
 */

const AWS = require("aws-sdk");

AWS.config.update({region: "us-east-1"});

exports.handler = async (event, context) => {
  const documentClient = new AWS.DynamoDB.DocumentClient({region: "us-east-1"});
  const { customAlphabet } = await import("nanoid");
  const nanoid = customAlphabet('0123456789abcdefghijklmnopqrstuvwxyz', 6);

  // console.log(event);

  let uid = nanoid();
  let date = new Date();

  const params = {
    TableName: "Share<###>DynUser<###>Detail-<id>-<stage>",
    Item: {
      id: event.request.userAttributes.sub,
      createdAt: date.toISOString(),
      updatedAt: date.toISOString(),
      __typename: "Share<###>DynUser<###>Detail",
      name: event.request.userAttributes.name,
      shortName: calculateShortName(event.request.userAttributes.name),
      uid: uid,
    }
  };

  try {
    const data = await documentClient.put(params).promise();
    console.log(data);
  } catch (err) {
    console.log(err);
  }

  return event;
};

function calculateShortName(fullName) {
  // Convert the full name to lowercase
  const lowerCaseName = fullName.toLowerCase();

  // Extract the first two letters
  const firstTwoLetters = lowerCaseName.substring(0, 2);

  // Find the index of the first space
  const firstSpaceIndex = lowerCaseName.indexOf(' ');

  // Extract the next two letters after the first space
  const nextTwoLetters = lowerCaseName.substring(firstSpaceIndex + 1, firstSpaceIndex + 3);

  // Concatenate the results to get the shortName
  const shortName = firstTwoLetters + nextTwoLetters;

  return shortName;
}
```

cd src folder of PostConfirmation function:

- npm install aws-sdk nanoid  
- amplify push

## Set up your frontend dependencies

Next, we’ll need to configure your React app to “be aware” of the Amplify-generated backend and configure a basic Amplify UI theme to make the UI components look great.

Replace your `index.js` file with the following code, which imports the Amplify libraries and connects them to the backend configuration (stored in aws-exports.js).
In addition, it’s configuring the default Amplify UI theme (the imported styles.css file and [<ThemeProvider />](https://ui.docs.amplify.aws/react/theming) for your <App /> component).

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { ThemeProvider } from '@aws-amplify/ui-react';
import '@aws-amplify/ui-react/styles.css';
import { Amplify } from 'aws-amplify'
import awsconfig from './aws-exports'

Amplify.configure(awsconfig)

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <ThemeProvider>
    <App />
  </ThemeProvider>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

```console
npm start
```

Test sign-up, check user is created in Cognito, added in authenticated group and added to User Detail table, sign-in, sign-out and then delete user:

- [Open ](https://localhost:3000/) in a standard browser window to set up the first user.  
- [Open ](https://localhost:3000/) in a private window to set up the second user.

## Create and display data records (todos) with GraphQL

[aws-amplify with the latest version (6.x.x) released, we no longer export the API class, but instead have a generateClient()](https://github.com/aws-amplify/amplify-js/issues/12635#:~:text=Hi%20%40greg%2Dmunro,our%20documentation.)
[Set up Amplify GraphQL API, Add your first record](https://docs.amplify.aws/react/build-a-backend/graphqlapi/set-up-graphql-api/#add-your-first-record)

Let’s first add a button to the app to create a new todo (content from trusty window.prompt). Import the Amplify library’s API category and createTodo GraphQL mutation in the App.js file (replace all in App.js with code below):

```js
import './App.css';

import {Amplify} from 'aws-amplify';
import {generateClient} from 'aws-amplify/api';
import {createShare<###>DynTo<###>Do} from './graphql/mutations';
import config from './amplifyconfiguration.json';

import "@aws-amplify/ui-react/styles.css";
import {withAuthenticator, Text, Flex, View, Badge, Button} from '@aws-amplify/ui-react';

Amplify.configure(config);

const client = generateClient();

function App({user, signOut}) {
  // console.log(user);
  return (
    <Flex direction={"column"} padding={8}>
      <Text>Logged in as <b>{user.signInDetails.loginId}</b> <Button variation='link' onClick={signOut}>Sign
        out</Button></Text>
      <Button onClick={() => {
        client.graphql({
          query: createShare<###>DynTo<###>Do,
          variables: {
            input: {
              content: window.prompt('content?'),
            }
          }
        });
      }}>Add todo
      </Button>
    </Flex>
  );
}

export default withAuthenticator(App);
```

Important: Remember when you configured “Amazon Cognito User Pool” earlier as the authorization mode earlier?

Amplify API will automatically now add your Amazon Cognito user’s username to the record as an “owner” because Amazon Cognito User Pool is the default authorization mode AND owner-based authorization is configured.

Next, let’s display your todos. Import the listTodos GraphQL queries from your src/graphql folder and the required React hooks:

Still in App.js:

```js
import {listShare<###>DynTo<###>Dos} from './graphql/queries'
import {useState, useEffect} from 'react'
```

Then, we need to fetch these todos when the App renders and display them. Create a state to store the todos and a useEffect to fetch the todos:

```js
..
function App({user, signOut}) {
  ..

  const [todos, setTodos] = useState([])

  useEffect(() => {
    const fetchTodos = async () => {
      const result = await client.graphql({ query: listShare<###>DynTo<###>Dos });
      // console.log(result);
      setTodos(result.data.listShare<###>DynTo<###>Dos.items)
    }

    fetchTodos()
  }, [])

  return (
    ..
  );
}
..
```

Next, display the fetched todos in the return statement of the App component. Add the following React code right below the “Add Todo” button component code:

```js
{todos.map(todo => <Flex direction="column" border="1px solid black" padding={8} key={todo.id}>
	<Text fontWeight={'bold'}>{todo.content}</Text>
	<View>👨‍👩‍👧‍👦 {todo.owners.map(owner => <Badge margin={4} key={owner}>{owner}</Badge>)}</View>
</Flex>)}
```

You should now be able to add new todos and upon refresh of the app see them listed. Try it out in your browser and go ahead and add a few to-dos and then refresh the page.

Now that we’ve got a few todos added, we also need the ability to delete them. Import the the deleteTodo GraphQL mutation and let’s add a button to delete a todo within each todo:

```js
import {createShare<###>DynTo<###>Do, deleteShare<###>DynTo<###>Do} from './graphql/mutations';
```

Now let’s add a delete button below the View component that displays all the to-do owners (inside {todos.map..}). (We’ll get to the “owners“ in a bit!):

```js
<Button onClick={async () => {
  client.graphql({
	query: deleteShare<###>DynTo<###>Do,
	variables: {
	  input: {
		id: todo.id
	  }
	}
  })
}}>Delete</Button>
```

Your App.js file should look something like this:

```js
import './App.css';

import {Amplify} from 'aws-amplify';
import {generateClient} from 'aws-amplify/api';
import {createShare<###>DynTo<###>Do, deleteShare<###>DynTo<###>Do} from './graphql/mutations';
import {listShare<###>DynTo<###>Dos} from './graphql/queries';
import {useState, useEffect} from 'react';
import config from './amplifyconfiguration.json';

import "@aws-amplify/ui-react/styles.css";
import {withAuthenticator, Text, Flex, View, Badge, Button} from '@aws-amplify/ui-react';

Amplify.configure(config);

const client = generateClient();

function App({user, signOut}) {
  // console.log(user);

  const [todos, setTodos] = useState([])

  useEffect(() => {
    const fetchTodos = async () => {
      const result = await client.graphql({query: listShare<###>DynTo<###>Dos});
      // console.log(result);
      setTodos(result.data.listShare<###>DynTo<###>Dos.items)
    }

    fetchTodos()
  }, [])

  return (
    <Flex direction={"column"} padding={8}>
      <Text>Logged in as <b>{user.signInDetails.loginId}</b> <Button variation='link' onClick={signOut}>Sign
        out</Button></Text>
      <Button onClick={() => {
        client.graphql({
          query: createShare<###>DynTo<###>Do,
          variables: {
            input: {
              content: window.prompt('content?'),
            }
          }
        });
      }}>Add todo
      </Button>
      {todos.map(todo => <Flex direction="column" border="1px solid black" padding={8} key={todo.id}>
        <Text fontWeight={'bold'}>{todo.content}</Text>
        <View>👨‍👩‍👧‍👦 {todo.owners.map(owner => <Badge margin={4} key={owner}>{owner}</Badge>)}</View>
        <Button onClick={async () => {
          client.graphql({
            query: deleteShare<###>DynTo<###>Do,
            variables: {
              input: {
                id: todo.id
              }
            }
          })
        }}>Delete</Button>
      </Flex>)}
    </Flex>
  );
}

export default withAuthenticator(App);
```

## Fetch real-time updates for records (todos) with GraphQL

GraphQL provides a mechanism called “subscriptions” to stream in real-time updates.
Amplify automatically creates subscriptions for record creation, updates, and deletions.
In your app code, you can then “apply” those updates to your existing/fetched data.

Let’s import the subscription statements for your GraphQL API into App.js:

```js
import { onCreateShare<###>DynTo<###>Do, onUpdateShare<###>DynTo<###>Do, onDeleteShare<###>DynTo<###>Do } from './graphql/subscriptions';
```

Next, we need to “subscribe” to those events and then apply the event inputs onto the fetched todos.
We’ll need to handle each case: create, update, and delete separately because they have different implications for the fetched todos.

[Subscribe to real-time events](https://docs.amplify.aws/react/build-a-backend/graphqlapi/subscribe-data/)

Let’s add all the subscription logic to your useEffect hook below your fetchTodos() function call:

```js
  useEffect(() => {
    const fetchTodos = async () => {
      const result = await client.graphql({query: listShare<###>DynTo<###>Dos});
      // console.log(result);
      setTodos(result.data.listShare<###>DynTo<###>Dos.items);
    }

    fetchTodos();

    // Subscriptions

    // Subscribe to creation of Todo
    const createSub = client
      .graphql({query: onCreateShare<###>DynTo<###>Do})
      .subscribe({
        // next: ({ data }) => console.log(data),
        next: ({data}) => {
          setTodos((todos) => [...todos, data.onCreateShare<###>DynTo<###>Do]);
        },
        error: (error) => console.warn(error)
      });

    // Subscribe to update of Todo
    const updateSub = client
      .graphql({query: onUpdateShare<###>DynTo<###>Do})
      .subscribe({
        // next: ({ data }) => console.log(data),
        next: ({data}) => {
          setTodos(todos => {
            const toUpdateIndex = todos.findIndex(item => item.id === data.onUpdateShare<###>DynTo<###>Do.id);
            if (toUpdateIndex === -1) { // If the todo doesn't exist, treat it like an "add"
              return [...todos, data.onUpdateShare<###>DynTo<###>Do];
            }
            return [...todos.slice(0, toUpdateIndex), data.onUpdateShare<###>DynTo<###>Do, ...todos.slice(toUpdateIndex + 1)];
          })
        },
        error: (error) => console.warn(error)
      });

    // Subscribe to deletion of Todo
    const deleteSub = client
      .graphql({query: onDeleteShare<###>DynTo<###>Do})
      .subscribe({
        // next: ({ data }) => console.log(data),
        next: ({data}) => {
          setTodos(todos => {
            const toDeleteIndex = todos.findIndex(item => item.id === data.onDeleteShare<###>DynTo<###>Do.id);
            return [...todos.slice(0, toDeleteIndex), ...todos.slice(toDeleteIndex + 1)];
          })
        },
        error: (error) => console.warn(error)
      });

    return () => {
      createSub.unsubscribe();
      updateSub.unsubscribe();
      deleteSub.unsubscribe();
    }
  }, [])
```

Make sure to return the unsubscribe() calls in the useEffect hook to clean-up the GraphQL subscriptions.

Important: When setting a React state from within a subscription make sure to use the “function” pattern of setState, where you can fetch the previous state.
setState((prevState) => newState) The subscription is set up in an useEffect hook, if you just reference todos without this function-based pattern, todos would always return an empty array.

Now, let’s try out your app locally. Every time you create or delete a todo, the changes are reflected in real-time.

## Fetch users from User Detail table

In App.js to fetch users, add code below on top of App function to firstly test log (uncomment console.log only to debug) fetch users from User Detail table:

```js
...
function App({user, signOut}) {
..

  const [users, setUsers] = useState([]);

  useEffect(() => {
    const fetchUsers = async () => {
      const result = await client.graphql({query: listShare<###>DynUser<###>Details});
      // console.log(result);
      setUsers(result.data.listShare<###>DynUser<###>Details.items);
    }

    fetchUsers();
  }, []);

  ..
}
..
```

## Make data records (todos) shareable with GraphQL

Let’s make the to-dos shareable! Remember the “owners” fields that we set up for the Todo model in the GraphQL schema?
Well we can append that with any user’s username to make them owners of a record as well!

Let’s import the updateTodo mutation into App.js:

```js
..
import {createShare<###>DynTo<###>Do, deleteShare<###>DynTo<###>Do, updateShare<###>DynTo<###>Do} from './graphql/mutations';
..
```

Now, let’s add a button (right before the delete button) to share the todo with other users:

```js
<Button onClick={async () => {
  client.graphql({
	query: updateShare<###>DynTo<###>Do,
	variables: {
	  input: {
		id: todo.id,
		owners: [...todo.owners, window.prompt('Share with whom?')]
	  }
	}
  });
}}>Share ➕</Button>
```

That’s it! Let’s try it now! Open two browser tabs (one in incognito) and login with two separate user accounts.
Then create a new todo and share it with the other user by just typing their username after clicking on the “Share button”.

## App.js end-to-end reference

Your App.js file should look something like the below in case you need an end-to-end reference:

```js
import './App.css';

import {Amplify} from 'aws-amplify';
import {generateClient} from 'aws-amplify/api';
import {createShare<###>DynTo<###>Do, deleteShare<###>DynTo<###>Do, updateShare<###>DynTo<###>Do} from './graphql/mutations';
import {listShare<###>DynTo<###>Dos, listShare<###>DynUser<###>Details} from './graphql/queries';
import {
  onCreateShare<###>DynTo<###>Do,
  onUpdateShare<###>DynTo<###>Do,
  onDeleteShare<###>DynTo<###>Do
} from './graphql/subscriptions';
import {useState, useEffect} from 'react';
import config from './amplifyconfiguration.json';

import "@aws-amplify/ui-react/styles.css";
import {withAuthenticator, Text, Flex, View, Badge, Button} from '@aws-amplify/ui-react';

Amplify.configure(config);

const client = generateClient();

function App({user, signOut}) {
  // console.log(user);

  const [todos, setTodos] = useState([]);
  const [users, setUsers] = useState([]);

  useEffect(() => {
    const fetchUsers = async () => {
      const result = await client.graphql({query: listShare<###>DynUser<###>Details});
      // console.log(result);
      setUsers(result.data.listShare<###>DynUser<###>Details.items);
    }

    fetchUsers();
  }, []);

  useEffect(() => {
    const fetchTodos = async () => {
      const result = await client.graphql({query: listShare<###>DynTo<###>Dos});
      // console.log(result);
      setTodos(result.data.listShare<###>DynTo<###>Dos.items);
    }

    fetchTodos();

    // Subscriptions

    // Subscribe to creation of Todo
    const createSub = client
      .graphql({query: onCreateShare<###>DynTo<###>Do})
      .subscribe({
        // next: ({ data }) => console.log(data),
        next: ({data}) => {
          setTodos((todos) => [...todos, data.onCreateShare<###>DynTo<###>Do]);
        },
        error: (error) => console.warn(error)
      });

    // Subscribe to update of Todo
    const updateSub = client
      .graphql({query: onUpdateShare<###>DynTo<###>Do})
      .subscribe({
        // next: ({ data }) => console.log(data),
        next: ({data}) => {
          setTodos(todos => {
            const toUpdateIndex = todos.findIndex(item => item.id === data.onUpdateShare<###>DynTo<###>Do.id);
            if (toUpdateIndex === -1) { // If the todo doesn't exist, treat it like an "add"
              return [...todos, data.onUpdateShare<###>DynTo<###>Do];
            }
            return [...todos.slice(0, toUpdateIndex), data.onUpdateShare<###>DynTo<###>Do, ...todos.slice(toUpdateIndex + 1)];
          })
        },
        error: (error) => console.warn(error)
      });

    // Subscribe to deletion of Todo
    const deleteSub = client
      .graphql({query: onDeleteShare<###>DynTo<###>Do})
      .subscribe({
        // next: ({ data }) => console.log(data),
        next: ({data}) => {
          setTodos(todos => {
            const toDeleteIndex = todos.findIndex(item => item.id === data.onDeleteShare<###>DynTo<###>Do.id);
            return [...todos.slice(0, toDeleteIndex), ...todos.slice(toDeleteIndex + 1)];
          })
        },
        error: (error) => console.warn(error)
      });

    return () => {
      createSub.unsubscribe();
      updateSub.unsubscribe();
      deleteSub.unsubscribe();
    }
  }, []);

  return (
    <Flex direction={"column"} padding={8}>
      <Text>Logged in as <b>{user.signInDetails.loginId}</b> <Button variation='link' onClick={signOut}>Sign
        out</Button></Text>
      <Button onClick={() => {
        client.graphql({
          query: createShare<###>DynTo<###>Do,
          variables: {
            input: {
              content: window.prompt('content?'),
            }
          }
        });
      }}>Add todo
      </Button>
      {todos.map(todo => <Flex direction="column" border="1px solid black" padding={8} key={todo.id}>
        <Text fontWeight={'bold'}>{todo.content}</Text>
        <View>👨‍👩‍👧‍👦 {todo.owners.map(owner => <Badge margin={4} key={owner}>{owner}</Badge>)}</View>
        <Button onClick={async () => {
          client.graphql({
            query: updateShare<###>DynTo<###>Do,
            variables: {
              input: {
                id: todo.id,
                owners: [...todo.owners, window.prompt('Share with whom?')]
              }
            }
          });
        }}>Share ➕</Button>
        <Button onClick={async () => {
          client.graphql({
            query: deleteShare<###>DynTo<###>Do,
            variables: {
              input: {
                id: todo.id
              }
            }
          })
        }}>Delete</Button>
      </Flex>)}
    </Flex>
  );
}

export default withAuthenticator(App);
```

[🥳 Success! Your app data is shareable and is real-time!](https://aws.amazon.com/fr/blogs/mobile/build-real-time-multi-user-experiences-using-graphql-on-aws-amplify/#:~:text=%F0%9F%A5%B3%20Success!%20Your%20app%20data%20is%20shareable%20and%20is%20real%2Dtime!)

---

## TODO

- [X] dynapp with pre sign-up process  
- [X] share-todo with front and realtime sharing mechanism  
- [_] retrieve todo author username from User table with Cognito user id  
- [_] share todo using username or uid (nano id of 6 characters)

---

## NEXT STEPS

- \<Authenticator\> .. \</Authenticator\> [SRC](https://stackoverflow.com/questions/70036160/amplifysignout-is-not-exported-from-aws-amplify-ui-react)
- \<AmplifySignOut \/> button [SRC](https://www.youtube.com/watch?v=Sk9HMuAaTmQ)
- post confirmation mail with "uid" [SRC](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-post-confirmation.html)

---
