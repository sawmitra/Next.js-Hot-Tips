# Optimize Next.js Builds: Remove Console Logs in Production

We can accidentally leak data and user sensitive information if we leave console logs in our code.

There is an easier way to remove them all or select few using the ```removeConsole``` option in the ```compiler``` object. You can remove console logs in Next.js at once by using the ```removeConsole``` option in the ```compiler``` object.

> [!TIP]
> Remember that this configuration only affects production builds. During development, all console logs will still be visible to aid in debugging.

## Demo

Let's say we have 2 buttons that log to the console when clicked, one with a ```console log``` and one with ```console.error```.

![Remove Console Logs](/asset/consoleLog/gif/RemoveConsoleLogs.gif)

In this demo, you can see the output of various console methods.

Now, let's implement the ```removeConsole``` option in our ```next.config.js``` file. If we want all console logs to be removed, we can do the following:

```js
// next.config.mjs

/** @type {import('next').NextConfig} */
const nextConfig = {
  compiler: {
    removeConsole: true,
  },
};

module.exports = nextConfig;
```

Here is a quick demo of the above configuration:

![Remove All Console Logs](/asset/consoleLog/gif/RemoveAllConsoleLogs.gif)

If we want to remove everything except keep specific console methods like ```console.error```, we can do so by using the ```exclude``` option.

```js
// next.config.mjs

/** @type {import('next').NextConfig} */
const nextConfig = {
  compiler: {
    removeConsole: {
      exclude: ['error'],
    },
  },
};

module.exports = nextConfig;
```

Here is a quick demo of the above configuration:

![Remove Console Logs Except Error](/asset/consoleLog/gif/RemoveConsoleLogsExceptError.gif)


## Here is the Code Playground for you to try it out

You might need to restart the development server for the changes to take effect.

[Open In New Tab](https://stackblitz.com/edit/stackblitz-starters-uww6ac?file=app%2Fpage.tsx)