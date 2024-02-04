# setup amplify env

Setup main (prod) / test / dev environnement for an AWS Amplify - React - project:

- main = production environnement.  
- test = environnement to share with stakeholder for testing, before going to prod.  
- dev = environnement to develop on feature branches.

Ref:

- [Team workflows with Amplify backend environments](https://docs.aws.amazon.com/amplify/latest/userguide/team-workflows-with-amplify-cli-backend-environments.html)  
- [Restricting access to branches](https://docs.aws.amazon.com/amplify/latest/userguide/access-control.html)  
- [Sandbox environments](https://docs.amplify.aws/javascript/tools/cli/teams/sandbox/)

Sandbox, github project [awsenvtest001](https://github.com/oldu73/awsenvtest001)

---

## Initialize React project

In terminal:

```console
npx create-react-app app1
cd app1
```

---

## AWS, main env

In terminal:

```console
amplify init

? Enter a name for the project app1
The following configuration will be applied:

? Initialize the project with the above configuration? No
? Enter a name for the environment main
? Choose your default editor: IntelliJ IDEA
√ Choose the type of app that you're building · javascript
Please tell us about your project
? What javascript framework are you using react
? Source Directory Path:  src
? Distribution Directory Path: build
? Build Command:  npm.cmd run-script build
? Start Command: npm.cmd run-script start
Using default provider  awscloudformation
? Select the authentication method you want to use: AWS profile
```

---

## Git, first step

Open project in your IDE, if using 'IntelliJ' like IDE (e.g. WebStorm) add './idea' folder to '.gitignore' file.

In terminal:

```console
git config user.email "john@doe.com"
git config user.name "John Doe"
git add .
git commit -m "initial commit"
```

---

## GitHub

Now go to your github account and create a new repository, e.g. "app1".

---

## Git, second step

```console
git branch -M main
git remote add origin https://github.com/oldu73/app1.git
git push -u origin main
```

---

## AWS, test, dev env

In terminal:

```console
amplify env add
 ? Do you want to use an existing environment? (Y/n): n
 ? Enter a name for the environment: test
...
amplify push

amplify env add
 ? Do you want to use an existing environment? (Y/n): n
 ? Enter a name for the environment: dev
...
amplify push
```

---

## Git commit dev and test env

In a terminal:

```console
git add .
git commit -m "added dev, and test environments"
git push origin main
```

---

## Amplify environment commands

Current:

```console
amplify status
```

List:

```console
amplify list env
```

Delete:

```console
amplify env remove dev
```

---

## Hosting environments

Switch to `Hosting environments` tab in AWS Amplify and connect corresponding git branch to environments.

In the build settings screen, pick an existing backend environment to set up continuous deployment with the main branches.

Grant the service role to Amplify. Choose Save and deploy.

After the build completes you will get branches deployments available at https:// `dev` or `test` or `main` .appid.amplifyapp.com.

---

## Password protect

[Protect deployed environments with password](https://docs.aws.amazon.com/amplify/latest/userguide/access-control.html)

---

## Feature branch

From dev:

```console
git checkout -b dev-<feature name>
amplify env checkout dev
amplify add api
...
amplify push
```

After you finish working on the feature, commit your code, create a pull request to review internally:

```console
git commit -m '<change description>'
git push origin dev-<feature name>
```

To preview what the changes will look like, go to the Amplify console and connect your feature branch. Note: If you have the AWS CLI installed on your system (Not the Amplify CLI), you can connect a branch directly from your terminal. You can find your appid by going to App settings > General > AppARN: arn:aws:amplify:<region>:<region>:apps/<appid>.

Maybe you should specify region like `--region us-east-1`:

```console
aws amplify create-branch --app-id <appid> --branch-name <branchname>
aws amplify start-job --app-id <appid> --branch-name <branchname> --job-type RELEASE
```

Check deployment status in aws amplify app web page and also backend name auto generated.

Once feature is OK, merge feature branch and then delete aws amplify branch.

!be careful, the auto-generated backend is not automatically deleted following the command above, so you have to do it manually!

---
