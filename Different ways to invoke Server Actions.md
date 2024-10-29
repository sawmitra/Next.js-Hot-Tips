# Different ways to invoke Server Actions

Let's talk about Server Actions in Next.js. You might think you need to use some fancy form action method to call these, but guess what? It's way simpler than that!

## Invoking Server Actions using Form action method

Sure, you can do it like this:

```js
// app/page.tsx

<form action={serverAction}>
  <button type="submit">Submit</button>
</form>
```

But there is another way. Sometimes, we want to perform more complex actions, like updating a todo item, showing a animation etc. for which we need to call server actions sequentially.

## Invoking Server Actions Sequentially

Check this out:

```js
// app/page.tsx

import { todoAction } from '@/actions/todo';
import { toast } from '@/components/ui/use-toast';

const handleFormAction = async (e) => {
  e.preventDefault();
  await todoAction();
  toast({ title: 'Todo item added' });
};

return (
  <form>
    <button onClick={handleFormAction}>Submit</button>
  </form>
);
```

See what we did there? We're just calling ```todoAction()``` like any other function.

Remember, Server Actions are just functions at the end of the day. So treat 'em like one! This approach gives you more flexibility and control over when and how you invoke your Server Actions.

So go ahead, call those Server Actions however you like.