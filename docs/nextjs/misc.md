# misc

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
