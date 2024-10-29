# Security Best Practices with Next.js

Next.js is a powerful framework for building web applications, but it's important to be aware of the security risks that come with it.

It is not the framework per se that is insecure, but the code that you write with it.

If you are not careful, you can introduce security vulnerabilities into your application. In today's hot tip, we will cover a few security risks that you might encounter when building your application with Next.js.

## Exposing Environment Variables

If you are using Next.js, you might be using environment variables to store your API keys, database credentials, and other sensitive information.

```
# .env

NEXT_PUBLIC_API_KEY=your-api-key
```

But prefixing the variable with ```NEXT_PUBLIC_``` makes it available to the client. This is not a good practice as it exposes your sensitive information to the client. Instead, you want to store the sensitive information on the server.

Try using Server Components to store the sensitive information on the server as Server Components are not rendered on the client. Similarly, you can use Server Actions to store the sensitive information on the server or even API Routes.

### When to use ```NEXT_PUBLIC_```?

Use ```NEXT_PUBLIC_``` when you want to expose the variable to the client. For example, analytics keys, Google Maps API keys, etc.

## Avoid Code Exposure

The lines between client and server are blurring with Next.js and overall web development. This means that you need to be careful about what code you expose to the client.

For example, if you export a function that fetches data and adds 'use server' at the top of the function, it will be exposed to the browser as an API endpoint. For example, fetchData here is exposed to the browser as an API endpoint.

```js
// actions/server-action.ts

'use server';

export async function fetchData() {
  const res = await fetch('https://api.users.com/data');
  return res.json();
}
```

> [!WARNING]  
> There is 1 thing wrong with this code.. Anyone can access this endpoint and see the users data.

What can you do? Well, you need to protect the endpoint by adding authentication checks just like you would do in any other application.

Try monitoring the network tab in your browser to see the API endpoints that are exposed. Now, functions are meant to be exposed to other files but with server actions, it trips a lot of developers up.

## Use 'server-only'

- Suggestion: Use 'server-only' to wrap the function to make it only available on the server.
- This would enforce the function to be only available on the server and not on the client. And not be exposed to the browser.
- This is a good practice to follow.
- You or your team won't accidentally expose the function to the client.

```js
import "server-only";
```

This would make the function only available on the server and not on the client. Since JavaScript modules can be shared between both Server and Client Components modules, it's possible for code that was only ever intended to be run on the server to sneak its way into the client.

These are just a few security risks but I'll be covering more in the [Modern Full Stack Next.js Course](https://nextjscourse.dev/).