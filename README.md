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

### CH. 8: Static and Dynamic Rendering
In the previous chapter, you fetched data for the Dashboard Overview page. However, we briefly discussed two limitations of the current setup:

The data requests are creating an unintentional waterfall.
The dashboard is static, so any data updates will not be reflected on your application.

What is Static Rendering?
With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or during revalidation. The result can then be distributed and cached in a Content Delivery Network (CDN).
Whenever a user visits your application, the cached result is served. There are a couple of benefits of static rendering:

Faster Websites - Prerendered content can be cached and globally distributed. This ensures that users around the world can access your website's content more quickly and reliably.
Reduced Server Load - Because the content is cached, your server does not have to dynamically generate content for each user request.
SEO - Prerendered content is easier for search engine crawlers to index, as the content is already available when the page loads. This can lead to improved search engine rankings.
Static rendering is useful for UI with no data or data that is shared across users, such as a static blog post or a product page. It might not be a good fit for a dashboard that has personalized data that is regularly updated.

What is Dynamic Rendering?
With dynamic rendering, content is rendered on the server for each user at request time (when the user visits the page). There are a couple of benefits of dynamic rendering:

Real-Time Data - Dynamic rendering allows your application to display real-time or frequently updated data. This is ideal for applications where data changes often.
User-Specific Content - It's easier to serve personalized content, such as dashboards or user profiles, and update the data based on user interaction.
Request Time Information - Dynamic rendering allows you to access information that can only be known at request time, such as cookies or the URL search parameters.

With dynamic rendering, your application is only as fast as your slowest data fetch.

### CH. 9: Streaming

#### What is streaming?
Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

##### loading.tsx
--loading.tsx is a special Next.js file built on top of Suspense, it allows you to create fallback UI to show 
  as a replacement while page content loads.
--Since <SideNav> is static, it's shown immediately. The user can interact with <SideNav> while the dynamic 
  content is loading.
--The user doesn't have to wait for the page to finish loading before navigating away (this is called 
  interruptable navigation).

#### Adding loading skeletons
A loading skeleton is a simplified version of the UI. Many websites use them as a placeholder (or fallback) to indicate to users that the content is loading. Any UI you embed into loading.tsx will be embedded as part of the static file, and sent first. Then, the rest of the dynamic content will be streamed from the server to the client.

##### Fixing the loading skeleton bug with route groups
Right now, your loading skeleton will apply to the invoices and customers pages as well.

Since loading.tsx is a level higher than /invoices/page.tsx and /customers/page.tsx in the file system, it's also applied to those pages.

We can change this with Route Groups. Create a new folder called /(overview) inside the dashboard folder. Then, move your loading.tsx and page.tsx files inside the folder:

Route groups allow you to organize files into logical groups without affecting the URL path structure. When you create a new folder using parentheses (), the name won't be included in the URL path. So /dashboard/(overview)/page.tsx becomes /dashboard.

Here, you're using a route group to ensure loading.tsx only applies to your dashboard overview page. However, you can also use route groups to separate your application into sections (e.g. (marketing) routes and (shop) routes) or by teams for larger applications.

#### Streaming a component
So far, you're streaming a whole page. But, instead, you can be more granular and stream specific components using React Suspense.

Suspense allows you to defer rendering parts of your application until some condition is met (e.g. data is loaded). You can wrap your dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads.

If you remember the slow data request, fetchRevenue(), this is the request that is slowing down the whole page. Instead of blocking your page, you can use Suspense to stream only this component and immediately show the rest of the page's UI.

#### Grouping components
Great! You're almost there, now you need to wrap the <Card> components in Suspense. You can fetch data for each individual card, but this could lead to a popping effect as the cards load in, this can be visually jarring for the user.

To create more of a staggered effect, you can group the cards using a wrapper component. This means the static <SideNav/> will be shown first, followed by the cards, etc.

#### Deciding where to place your Suspense boundaries
Where you place your Suspense boundaries will depend on a few things:

--How you want the user to experience the page as it streams.
--What content you want to prioritize.
--If the components rely on data fetching.

### CH. 10: Partial Prerendering (beta)
Combining Static and Dynamic Content
Currently, if you call a dynamic function inside your route (e.g. noStore(), cookies(), etc), your entire route becomes dynamic.

This is how most web apps are built today. You either choose between static and dynamic rendering for your entire application or for a specific route.

However, most routes are not fully static or dynamic. You may have a route that has both static and dynamic content. For example, consider an ecommerce site. You might be able to prerender the majority of the product page, but you may want to fetch the user's cart and recommended products dynamically on-demand.