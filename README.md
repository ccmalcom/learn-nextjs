## Next.js App Router Course - Starter

This is the starter template for the Next.js App Router Course. It contains the starting code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.


### CH. 3: Optimizing Images & Font
Image Optimization is a large topic in web development that could be considered a specialization in itself. Instead of manually implementing these optimizations, you can use the next/image component to automatically optimize your images.\

The <Image> Component is an extension of the HTML <img> tag, and comes with automatic image optimization, such as:

Preventing layout shift automatically when images are loading.
Resizing images to avoid shipping large images to devices with a smaller viewport.
Lazy loading images by default (images load as they enter the viewport).
Serving images in modern formats, like WebP and AVIF, when the browser supports it.

-It's good practice to set the width and height of your images to avoid layout shift, these should be an aspect ratio identical to the source image.

-Web fonts can also cause layout shift-- load fonts using next.js 

### CH. 4: Routing
Next.js uses file-system routing where folders are used to create nested routes. Each folder represents a route segment that maps to a URL segment.

You can create separate UIs for each route using layout.tsx and page.tsx files.

page.tsx is a special Next.js file that exports a React component, and it's required for the route to be accessible.

layout.tsx
-The <Layout /> component receives a children prop. This child can either be a page or another layout. In this app's case, the pages inside /dashboard will automatically be nested inside a <Layout /> 

-One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called partial rendering

/app/layout.tsx: This is called a root layout and is required. Any UI you add to the root layout will be shared across all pages in your application. You can use the root layout to modify your <html> and <body> tags, and add metadata

### CH. 5: Navigation and Links