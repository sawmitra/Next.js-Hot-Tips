# The Magic Behind Instant Cart Updates

When building e-commerce applications or any system that requires real-time updates, it's crucial to provide instant feedback to users. In a traditional React application, you might use client-side state management to update the cart count immediately.

However, with Next.js Server Components and Server Actions, we can achieve the same result with server-side rendering, ensuring that the data is always fresh and consistent.

Let's explore how this works with an interactive demo. These are products from one of my favourite stores, [I Hate Copy](https://hatecopy.com/collections/prints). I infact own the "Hot Chai Cold Revenge" print.

## Demo: With Revalidate

- Try adding items to the cart and see the magic happen.
- You won't need to refresh the page to see the updated cart count as the page is revalidated.

[Demo](https://www.nextjscourse.dev/courses/nextjs-hot-tips/magic-behind-instant-cart-updates)

- This demo uses real Server Actions to update the cart count.
- The cart count is stored on the server and updated instantly.

## How it works

- **Server Components**: The product list and initial cart data are rendered on the server. This ensures that the initial page load is fast and SEO-friendly.

- **Server Actions**: When a user clicks "Add to Cart", a server action is triggered. This action updates the cart data on the server.

- **Revalidation**: After the server action completes, Next.js can automatically revalidate the affected components if we use ```revalidatePath()```. This means the cart data is updated server-side and the new data is sent to the client.

- **Instant Updates**: The client receives the updated cart data and rerenders only the necessary parts of the page, providing an instant update to the user.

## The difference between using revalidatePath() and not using it

In the demo above, you can switch between two modes:

- **With Revalidate**: This mode uses ```revalidatePath()``` in the server action. When you add an item to the cart, the entire page is revalidated, ensuring that all components have the most up-to-date data.

- **Without Revalidate**: This mode doesn't use ```revalidatePath()```. When you add an item to the cart, only the specific component that called the server action will update. Other components on the page might still show stale data.

Now that you have tried the "with revalidate" demo, try adding items to the cart for the demo below "Without Revalidate", you'll notice that in "With Revalidate" mode, the cart contents always reflect the server state however in "Without Revalidate" mode, you might need to refresh the page to see the updated cart contents if there are other components on the page displaying cart data.

## Demo: Without Revalidate

> [!WARNING]
> You won't see the cart update in this mode as the page is not revalidated. You'll need to refresh the page to see the updated cart count.

[Demo](https://www.nextjscourse.dev/courses/nextjs-hot-tips/magic-behind-instant-cart-updates)

- This demo uses real Server Actions to update the cart count.
- The cart count is stored on the server and updated instantly.

## How to implement this in your own app

### Server Action Code

Here's the Server Action code that updates the cart count and revalidates the page:

```js
// actions/cart-actions.ts

'use server';

import { revalidatePath } from 'next/cache';

let cartCount = 0; // This simulates a database. In a real app, you'd use a database instead.

export async function addToCart(productId: number) {
  // Simulate a delay, as if we're writing to a database
  await new Promise((resolve) => setTimeout(resolve, 500));

  // Update the cart count
  cartCount++;

  // Revalidate only the cart count component
  revalidatePath('/', 'page'); 

  return cartCount;
}

export async function getCartCount() {
  // In a real app, you'd fetch this from a database
  return cartCount;
}
```

### AddToCart Component

Here is the code for the AddToCart component:

```js
// components/cart/add-to-cart.tsx

'use client';

import { addToCart } from '../actions/cartActions';
import { Button } from '@/components/ui/button';
import { RefreshCw } from 'lucide-react';
import React, { useState, useCallback } from 'react';

export default function AddToCart({ productId }: { productId: number }) {
  const [isUpdating, setIsUpdating] = useState(false);

  const handleAddToCart = useCallback(async () => {
    if (isUpdating) return; // Prevent multiple clicks while updating

    setIsUpdating(true);
    try {
      await addToCart(productId); 
    } catch (error) {
      console.error('Failed to add item to cart:', error);
    } finally {
      setIsUpdating(false);
    }
  }, [productId, isUpdating]);

  return (
    <Button
      onClick={handleAddToCart}
      disabled={isUpdating}
      className="bg-pink-600 hover:bg-pink-700 text-white"
    >
      {isUpdating ? (
        <RefreshCw className="w-4 h-4 animate-spin" />
      ) : (
        'Add to Cart'
      )}
    </Button>
  );
}
```