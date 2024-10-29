# Next.js Server Components vs Server Actions: A Comprehensive Guide

Server Components and Server Actions are two different ways to handle server-side rendering in Next.js.

Server Components are meant to fetch data and render the UI on the server. Server Actions on the other hand are meant to perform mutations on the server. You won't be able to understand Server Components and Server Actions if you don't understand Server Rendering.

## What is Server Rendering?

Server-side Rendering (SSR) involves pre-generating the HTML on the server on page load and sending it to the client. Once the client receives the bundle, it will download that bundle, page is hydrated and make the page interactive as JavaScript is executed.

## What are Server Components?

Server Components are React components that are rendered on the server. If they are rendered on the server so are they the same as Server Rendering?

No, they are not the same. Server Components on the other hand is pre-generating the HTML and rendering on the server and it sends the browser a special stream for browser to show on the page.

Now, Server Components never re-render as they don't store any client-side state.

## What are Server Actions?

Server Actions are functions that are executed on the server. They are wrappers around API Routes and can be used to perform mutations on the server. They give us the ability to perform mutations on the server. If you have a form, you can use a Server Action to handle the form submission.

> [!TIP]
> If there is one thing you need to take away from this hot tip, it is that you need to fetch data with Server Components i.e. GET requests and perform mutations with Server Actions i.e. POST, PUT, DELETE requests.

## Difference between Server Components and Server Actions

- Server Components are rendered and executed on the server whereas Server Actions are executed on the server and are used to perform mutations.
- Server Actions functions get exposed to the client as an API Route whereas Server Components are not rendered on the client, they are rendered on the server and return a special stream for the browser to show on the page.

This topic is huge and we will cover it in detail in the [Modern Full Stack Next.js course](https://www.nextjscourse.dev/).