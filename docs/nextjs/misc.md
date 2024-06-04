# misc

---

## Which kind of Next to choose?

Pages router = old  
App router = new

App router client VS server components then depends on usage.

Which Next.js to choose?:

- [App Router vs Pages Router](https://nextjs.org/docs#:~:text=the%20Installation%20guide.-,App%20Router%20vs%20Pages%20Router,-Next.js%20has)  
- [Client VS Server Components](https://nextjs.org/docs/app/building-your-application/rendering)

---

## Setup new project

```console
C:\>npx create-next-app@latest
√ What is your project named? ... my-app
√ Would you like to use TypeScript? ... No
√ Would you like to use ESLint? ... Yes
√ Would you like to use Tailwind CSS? ... Yes
√ Would you like to use `src/` directory? ... Yes
√ Would you like to use App Router? (recommended) ... Yes
√ Would you like to customize the default import alias (@/*)? ... No
Creating a new Next.js app in C:\my-app.
```

```console
npm run dev
```

[localhost:3000]( http://localhost:3000)

---

## First route

In app folder create a `hello` folder and in it a `route.js` with content:

```js
export async function GET(request){
  return new Response('Hello, world!');
}
```

Test it: [localhost:3000/hello](http://localhost:3000/hello)

---

## Second route

In app folder create a `dashboard` folder and in it a `page.js` with content:

```js
export default function DashboardPage() {
  return (
    <div className="dashboard-page">
      <p>Hello, dashboard !</p>
    </div>
  )
}
```

Test it: [localhost:3000/dashboard](http://localhost:3000/dashboard)

---

## Debug

Debug in VS Code:

- [documentation explains how debug Next.js](https://nextjs.org/docs/pages/building-your-application/configuring/debugging)  
- [How to Debug Next.js 13 and React in VS Code](https://www.youtube.com/watch?v=bqk3Rnsr5gU)

---
