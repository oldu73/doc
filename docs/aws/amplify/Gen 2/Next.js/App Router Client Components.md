# App Router Client Components

Ref. [Next.js App Router (Client Components)](https://docs.amplify.aws/gen2/start/quickstart/nextjs-app-router-client-components/)

---

## prerequisites

install:

- Node.js v18.17 or later  
- npm v9 or later  
- git v2.14.1 or later  
- AWS account

## setup

create project:

```console
npm create next-app@14 -- next-amplify-gen2 --typescript --eslint --app --no-src-dir --no-tailwind --import-alias '@/*'
cd next-amplify-gen2
npm create amplify@latest
? Where should we create your project? (.) # press enter
npm run dev
```

Open [localhost:3000](http://localhost:3000/) in a browser to check server is running.

Hit `Ctrl + c` in command prompt window to stop dev server and open project in IDE.

Setup `git config user` email and name parameters.

Add `.idea` folder to `.gitignore` file in case of using JetBrains IDE like WebStorm.

---

## sandbox

[sandbox CLI commands](https://docs.amplify.aws/gen2/reference/cli-commands/)

In terminal:

```console
npm run dev
npx amplify sandbox
```

Hit `Ctrl + c` in sandbox terminal to stop, you may choose to keep it for further usage.

.. [wipp](https://docs.amplify.aws/gen2/start/quickstart/nextjs-app-router-client-components/#build-a-backend)

---

...

---

Deploy amplify gen 2 be aware of !Next.js 14 apps require a minimum node version of 18.17 at build time.

Under Advanced settings, navigate to Build Image and enter `amplify:al2023`.  
The Amplify Amazon Linux 2023 build image supports Node.js versions 18 and 20 at build time.  
You can specify the Node.js version during your build via live package updates or nvm.  
Amplify Hosting will automatically deploy your compute runtime to match the Node.js version used at build time.

---
