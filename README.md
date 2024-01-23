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
In Next.js, you can use the <Link /> Component to link between pages in your application. <Link> allows you to do client-side navigation with JavaScript.

Automatic code-splitting and prefetching
To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

Futhermore, in production, whenever <Link> components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

### CH.6: deploy to vercel, seed db data (postgres)

### CH. 7: Fetching Data
Choosing how to fetch data
API layer
-APIs are an intermediary layer between your application code and database. There are a few cases where you might use an API:

--If you're using 3rd party services that provide an API.
--If you're fetching data from the client, you want to have an API layer that runs on
  the server to avoid exposing your database secrets to the client.
--In Next.js, you can create API endpoints using Route Handlers.

Database queries
-When you're creating a full-stack application, you'll also need to write logic to interact with your database. For relational databases like Postgres, you can do this with SQL, or an ORM like Prisma.

There are a few cases where you have to write database queries:

--When creating your API endpoints, you need to write logic to interact with your 
  database.
--If you are using React Server Components (fetching data on the server), you can 
  skip the API layer, and query your database directly without risking exposing your database secrets to the client.

Using Server Components to fetch data
By default, Next.js applications use React Server Components. Fetching data with Server Components is a relatively new approach and there are a few benefits of using them:

--Server Components support promises, providing a simpler solution for asynchronous 
  tasks like data fetching. You can use async/await syntax without reaching out for useEffect, useState or data fetching libraries.
--Server Components execute on the server, so you can keep expensive data fetches and 
  logic on the server and only send the result to the client.
--As mentioned before, since Server Components execute on the server, you can query 
  the database directly without an additional API layer.
