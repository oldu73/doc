# 02-amplify-setup

from app's root folder

---

## Create an Amplify backend for the app

```console
amplify init
```

---

## Create resources for categories

- Amplify Auth: allows users to sign up, sign in, and manage their account
- Amplify API: allows users to create, read, update, and delete trips
- Amplify Storage: allows users to upload and view images in their app

```console
amplify push
```

---

## Allow access for data content in Amplify Studio

[SRC: aws](https://docs.amplify.aws/console/data/content-management/#iam-required-as-auth-provider)

```console
amplify update api

Select GraphQL
Select Authorization Modes
Choose the default authorization type for the API Amazon Cognito User Pool
Select Configure additional auth types
Select IAM

amplify push
```

---

## After modifying data model

in schema.graphql file (amplify/backend/api/amplifytripsplanner/schema.graphql), then

```console
amplify codegen models
amplify push
```

---

## Fail to push

amplify push fail due to yarn deprecated issue

Did it from WSL but was certainly working most likely because CLI was older than the Windows's one.

I suspect this issue to appear after an amplify cli update on Windows.. but not sure.. anyway..

fixed by launching commands from WSL (directly in Windows folder (/mnt/c/git/..)) instead of command.prompt:

- git clone  
- amplify init  
- amplify push

Then you may switch to Windows environnement without any problem.

---

## Connect app to existing backend

If a backend is already deployed and you want to connect your app to it.

In amplify, click on your backend (app/amplify project/etc.), then expand `Edit backend` section then you have local setup instructions.

Connect your app to this backend environment using the Amplify CLI by running the following command from your project root folder, e.g.:

```console
amplify pull --appId d2f8n0exni69yo --envName dev
```

---

## Clean up resources

When the project will no longer be useful, to avoid incurring unexpected costs.

!Warning, all aws infrastructure, data and files will be lost!

```console
amplify delete
```

!! NEVER GIT PUSH PROJECT AFTER AMPLIFY DELETE!!

Just remove project's folder locally and git pull again (then amplify init/pull or connect app to existing backend) on need to work again on the project.

---
