# Streamlining Redirects for Better Performance

Redirects in Server Actions allow you to redirect the user after a mutation is successful. They enable server-side redirects instead of passing a status code to the client and letting the client handle the redirect.

## Understanding Server-side vs Client-side Redirects

When working with Next.js, it's important to understand the difference between server-side and client-side redirects.

Let's explore this concept with an interactive demo. Press the button "Simulate Redirect" to see the difference between server-side and client-side redirects.

### Key Differences:

- Execution: Server-side redirects occur immediately after a server action completes, while client-side redirects happen in the browser after the JavaScript bundle is loaded and executed.

- Performance: Server-side redirects can be faster, especially on slower devices or networks, as they don't require additional JavaScript execution on the client.

- SEO: Server-side redirects are generally better for SEO, as search engine crawlers can follow them more easily.

- User Experience: Server-side redirects can provide a smoother experience, particularly when navigating between different sections of your application and are better for SEO.

## Implementing Server-side Redirects in Next.js

In Next.js, you can implement server-side redirects using the ```redirect``` function from ```next/navigation```. Here's an example of how to use it in a Server Action:

Here we are adding products to the database and then redirecting the user to the products view page after we have validated the form data.

> [!WARNING]  
> If we redirect inside a try catch block, **the redirect will not work**. Here's why, because redirect throws an error and the catch block will catch the error and return a response which in this case log the error and return a error type response.

If we redirect outside the try catch block, the redirect will work as there is no try catch block to catch the error.

### Before

```js
// actions/product-actions.ts

import { redirect } from 'next/navigation';
import { storeDataInit } from '@/lib/actions/storeDataInit';

export async function addProductAction(prevState: State, formData: FormData) {
  const { name, quantity } = validatedFields.data;
  try {
    if (name && quantity) {
      await storeDataInit({ name: name.toString(), quantity });
      redirect('/products/view'); 
    }
  } catch (error) {
    console.log({ error });
    return {
      type: 'error',
      message: 'Error: Failed to Create Inventory',
    };
  }
}
```

### After

```js
// actions/product-actions.ts

import { redirect } from 'next/navigation';
import { storeDataInit } from '@/lib/actions/storeDataInit';

export async function addProductAction(prevState: State, formData: FormData) {
 //validatedFields are form fields
  const { name, quantity } = validatedFields.data;
  try {
    if (name && quantity) {
      await storeDataInit({ name: name.toString(), quantity });
    }
  } catch (error) {
    console.log({ error });
    return {
      type: 'error',
      message: 'Error: Failed to Create Inventory',
    };
  }
  redirect('/products/view'); 
}
```

## Conclusion

In this lesson, we've explored the importance of server-side redirects in Next.js and how they can enhance user experience and SEO. We've also seen how to implement these redirects using the ```redirect``` function from ```next/navigation```.

By understanding and utilizing server-side redirects, you can create a more seamless and efficient user experience in your Next.js applications.