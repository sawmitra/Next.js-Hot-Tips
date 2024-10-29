# Next.js Image Optimization Performance

Performance is crucial in Next.js, and the framework provides many built-in components to help. One of the most powerful is the Image component, which offers:

- Automatic image optimization
- Responsive image handling
- Lazy loading
- And more!

## The Next.js Image Component

Next.js recommends using the [Image component](https://nextjs.org/docs/app/api-reference/components/image) whenever you would typically use an ```<img>``` tag. This component enhances your images with powerful optimizations.

## The ```sizes``` Prop

While the Image component is great on its own, it becomes even more powerful when you utilize the ```sizes``` prop. This prop isn't unique to Next.js; it's actually a standard HTML attribute for ```<img>``` elements.

Here's an example of using the Image component with the ```sizes``` prop:

```javascript 
// app/page.tsx

import Image from 'next/image';
import profilePic from '../public/og-hot-tips.png';

export default function Home() {
  return (
    <main className="p-4 flex flex-col items-center justify-center">
      <h1 className="text-2xl font-bold mb-4">
        Next.js Image Component with sizes prop
      </h1>
      <Image
        src={profilePic}
        alt="Example image"
        width={900}
        height={400}
        **`sizes="(max-width: 768px) 100vw, (max-width: 900px) 50vw, 33vw"`**
      />
      <p className="mt-4">
        The image above uses the sizes prop to optimize loading based on
        viewport size.
      </p>
    </main>
  );
}
```

> [!NOTE]  
> The [Image component](https://nextjs.org/docs/app/api-reference/components/image) uses the sizes prop to determine the size of the image to load. It does this by using the width and height props to calculate the appropriate image size.

## Try it out yourself

In the app, you will see an image of the banner for this course. We are going to open up browser developer tools and learn about the sizes prop.

[Open In New Tab](https://stackblitz.com/edit/stackblitz-starters-mdqrub?file=app%2Fpage.tsx)

### To see this in action:

In this example, it is best for you to open the code sample in a new tab.

- Go to the StackBlitz Project link above in a new tab

- Open Browser's developer tools of the StackBlitz project

    - In Chrome, you can do this by right clicking on the page and selecting "Inspect"
- Go to the Network tab

- Reload the page

- Observe how images are loaded and optimized especially the size of the image downloaded.

- Note the image size on desktop and compare that to mobile. (Refer the screenshot below to see where to look)

### Here's what's going on:

- As you resize the browser window, the image size changes (make sure to reload the page after resizing)

- Images are loaded lazily as they enter the viewport

- They're served in modern formats like WebP when supported

- Images are resized to fit the container and device

But, Notice, how the image size is different on desktop: 19.2 KB and mobile: 16 KB.

I know what you're thinking, "That's a small difference!". Well, you are right! Although, imagine if you had 100 images on the page. The difference would add up! The Web is full of images and every byte saved counts!

This would get worse on slow networks 🐌.

When I travel, I am often reminded of how important performance is when I am on a slow network 😅.

### Best Practices for Using the Image Component

- Always provide ```width``` and ```height``` to prevent layout shift
- Use the ```priority``` prop for above-the-fold images
- Utilize the ```sizes``` prop for responsive designs
- Opt for static imports for local images when possible

### Conclusion
The Next.js Image component, especially when used with the sizes prop, is a powerful tool for optimizing images in your web applications. By using it effectively, you can significantly improve your site's performance and user experience.

Remember to experiment with different settings and always test on various devices to ensure optimal performance across all screen sizes.

Happy coding!