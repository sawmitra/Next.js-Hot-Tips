# Visual Guide to Next.js Caching

Understanding caching in Next.js is crucial for performance. Let me show you how to visualize it using Chrome DevTools and a custom interactive component.

- SSG (Static Site Generation): Data is fetched at build time and remains static.
- ISR (Incremental Static Regeneration): Data is initially static but refreshes at regular intervals.
- SSR (Server-Side Rendering): Fresh data is fetched on every request.

> [!NOTE] 
> Also, note the cache status of each request. You'll see cache as HIT when the data is served from the cache, MISS when the data is not in the cache, and STALE when the data is in the cache but outdated.

### Cache States: The Basics
In Next.js, a cached request falls into one of these categories:

- HIT: Served from cache. Fast and efficient.
- MISS: Not in cache. New request made.
- STALE: In cache, but outdated.

Knowing these helps you debug and optimize your fetch requests.

### Visualizing Cache with Chrome DevTools
Here's a quick way to see cache behavior in real applications:

- Open Chrome DevTools (F12)
- Go to Network tab
- Right-click in the request list area
- Navigate to "Header options" > "Response Headers" > "Manage Header Options"
- Click "Add custom Header" and add "x-vercel-cache"
- Enable "Cache Control" too

Now you'll see cache status and control info for each request.

Here's what it looks like:

![cache_demo](/cache_eybnaq.webp)

You'll see those as columns in the Network tab.

> [!TIP]
> Heads up: Next.js 14 caches by default without a specified stale time. So your data being stale despite page refresh might trip you up. Next.js 15 is changing this to no-store by default (Server Rendering) based on community feedback.

### Wrap Up

Visualizing cache behavior is key to optimizing your Next.js app. Use these tools to balance fresh data and performance.