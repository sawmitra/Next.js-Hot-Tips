# Secure Form Handling: Data Sanitization in Next.js

Data sanitization is the process of removing or modifying data to ensure it is safe and secure. This is important to prevent attacks such as SQL injection, cross-site scripting (XSS), and other types of attacks.

Before we send data to the server, we need to sanitize it. What does sanitizing data mean? It means removing or modifying unnecessary characters or data that could be harmful to the server or other users. This step gives us confidence that the data we are sending to the server is safe and secure.

There are several strategies to sanitize data:

- **Validation**: This is the process of checking if the data is in the correct format. For example, we can check if the data is a number, a string, or a boolean.
- **Sanitization**: This is the process of removing or modifying data to ensure it is safe and secure. For example, we can remove or modify data that could be harmful to the server or other users.
- **Escaping**: This is the process of converting special characters to their corresponding HTML entities. This is important to prevent attacks such as XSS.

and more...

When you are dealing with forms, you are dealing with users that can enter whatever they want. This is where sanitization comes in effect. You need to validate and sanitize the data to ensure it is safe and secure.

To do this, we can use a library called Zod.

## What is Zod?

Zod is a TypeScript-first schema validation library that allows you to define a schema for your data. It's a powerful tool for validating data from users and API’s.

Because we are defining a schema i.e. a structure that the data needs to abide to, Zod will prevent any data that does not match to the schema from being sent to the server. It will also give you a clear error message of what is wrong with the data.

## Let's define a schema for a form

Let's say we have a form that allows users to enter their name, email, and age. We can define a schema for this form as follows:

```js
const userSchema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
  age: z.number().min(18),
});
```

In the above schema, we are defining a structure that there will be 3 fields: name, email, and age, the type of each field and the rules that each field needs to abide to. For instance,

- **name**: The name field is a string that must be at least 1 character long.
- **email**: The email field is a string that must be a valid email address.
- **age**: The age field is a number that must be at least 18.
If a user enters data that does not match the schema, Zod will throw an error which we can define as well.

## Let's see how this works in practice

```js
const userSchema = z.object({
  name: z.string().min(1).max(20).error('Name must be between 1 and 20 characters'),
  email: z.string().email().error('Email is not valid'),
  age: z.number().min(18).max(100).error('Age must be between 18 and 100'),
});

const user = userSchema.parse({
  name: 'John Doe',
  email: 'john.doe@example.com',
  age: 20,
});

console.log(user);
```
By adding ```.error('Error message')```, we are defining what error message to show if the data does not match the schema.

```userSchema.parse()``` will parse the data into the schema and return the data if it is valid. If the data is not valid, it will throw an error. If you try to another another field in the form, it won't let it through.

## Using Zod in Next.js

Next.js works great with Zod. You can use the ```zodResolver``` to resolve the schema to the form fields. If you are using React Hook Form, you can use the ```zodResolver``` to resolve the schema to the form fields.

## Data Sanitization Demo with Zod and React Hook Form
- Try changing the values in the form and see what happens on the right hand side. In Sanitized Data, you will see the data that is being parsed by Zod.
- In Form Errors, you will see the error messages that Zod throws as the user enters invalid data.

[Data Sanitization Demo](https://www.nextjscourse.dev/courses/nextjs-hot-tips/data-sanitization-with-forms)

## Try it out yourself

If you would like to try out the code, you can press Run Project.

[Data Sanitization Demo with Zod and React Hook Form](https://stackblitz.com/edit/stackblitz-starters-zgsqc7?file=app%2Fpage.tsx)

## Here is how the code works:

Let's break down the key components of the ```DataSanitizationDemo```:

### Schema Definition

We define a Zod schema that specifies the structure and validation rules for our form data:

```js
const formSchema = z.object({
  username: z
    .string()
    .min(3, { message: 'Username must be at least 3 characters long' })
    .max(20, { message: 'Username must not exceed 20 characters' })
    .transform((value) => value.toLowerCase().trim()),
  // ... other fields ...
});
```

This schema not only validates the input but also sanitizes it (e.g., trimming whitespace and converting to lowercase).

### Form Setup

We use ```react-hook-form``` with Zod integration to create a form controller. zodResolver is used to resolve the schema to the form fields.

```js
const form = useForm<FormValues>({
  resolver: zodResolver(formSchema),
  defaultValues: {
    username: '',
    email: '',
    age: undefined,
    bio: '',
  },
  mode: 'onChange',
});
```

This setup ensures that our form uses Zod for validation and updates on every change.

### Real-time Sanitization

We use a ```useMemo``` hook to attempt parsing the form values through our Zod schema in real-time:

```js
const [sanitizedData, isValid] = React.useMemo(() => {
  try {
    const parsed = formSchema.parse(watchedValues);
    return [parsed, true];
  } catch (error) {
    return [watchedValues, false];
  }
}, [watchedValues]);
```

This gives us constantly updated, sanitized data and a validity status.

### Form Rendering

We use UI components from [ShadCN UI](https://ui.shadcn.com/) to render the form fields, each connected to our form controller. The form is connected to the form controller and the form state by react-hook-form.

```js
import {
  FormField,
  FormItem,
  FormLabel,
  FormControl,
  FormDescription,
  FormMessage,
} from '@/components/ui/form';

<FormField
  control={form.control}
  name="username"
  render={({ field }) => (
    <FormItem>
      <FormLabel>Username</FormLabel>
      <FormControl>
        <Input placeholder="johndoe" {...field} />
      </FormControl>
      <FormDescription>Enter a username (3-20 characters)</FormDescription>
      <FormMessage />
    </FormItem>
  )}
/>;
```

### Displaying Results

We show the sanitized data and any form errors in real-time:

```js
<pre className={/* ... */}>
  {JSON.stringify(sanitizedData, null, 2)}
</pre>

<pre className={/* ... */}>
  {JSON.stringify(form.formState.errors, null, 2)}
</pre>
```

By using Zod with react-hook-form, we create a robust system for handling user input, ensuring that only clean, validated data makes it through to our application logic.