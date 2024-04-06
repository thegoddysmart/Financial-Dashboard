# Financial Dashboard Project


## What I will be building
I will be building a simplified version of the financial dashboard that has: 
- A public home page
- A login page
- Dashboard pages that are protected by authentication
- The ability for users to add, edit, and delete invoices

The dashboard will also have an accompanying database


## Overview
Here's an overview of features I'll learn about in this project:

- Styling: The different ways to style my application in Next.js.
- Optimizations: How to optimize images, links, and fonts.
- Routing: How to create nested layouts and pages using file-system routing.
- Data Fetching: How to set up a database on Vercel, and best practices for fetching and streaming.
- Search and Pagination: How to implement search and pagination using URL Search Params.
- Mutating Data: How to mutate data using React Server Actions, and revalidate the Next.js cache.
- Error Handling: How to handle general and 404 not found errors.
- Form Validation and Accessibility: How to do server-side form validation and tips for improving accessibility.
- Authentication: How to add authentication to my application using NextAuth.js and Middleware.
- Metadata: How to add metadata and prepare my application for social sharing.


## System requirements
Before you start this project, make sure our system meets the following requirements:

- Node.js 18.17.0 or later installed. Download here.
- Operating systems: macOS, Windows (including WSL), or Linux.


# Chapter One

## Folder Structure
- `/app`: Contains all the routes, components, and logic for this application, this is where I'll be mostly working from.
- `/`app/`lib`: Contains functions used in this application, such as reusable utility functions and data fetching functions.
- `/`app/`ui`: Contains all the UI components for this application, such as cards, tables, and forms.
- `/public`: Contains all the static assets for this application, such as images.
- `/scripts`: Contains a seeding script that I'll use to populate my database.
- Config Files: Most of these files are created and pre-configured when I start a new project using create-next-app.


## Placeholder data
When building user interfaces, it helps to have some placeholder data. If a database or API is not yet available, one can:

- Use placeholder data in JSON format or as JavaScript objects.
- Use a 3rd party service like mockAPI.

For this project, some placeholder data in ``app/`lib/placeholder-data.js`. Each JavaScript object in the file represents a table in the database.

## TypeScript
Most files have a .ts or .tsx suffix.

By using TypeScript, I can ensure I don't accidentally pass the wrong data format to my components or database, like passing a string instead of a number to invoice amount.


## Running the development server
Run `npm i` to install the project's packages.

Followed by `npm run dev` to start the development server.

Open `http://localhost:3000` on our browser. 


# Chapter 2

## CSS Styling
- How to add a global CSS file to our application.

- Two different ways of styling: Tailwind and CSS modules.

- How to conditionally add class names with the clsx utility package.

### Global Styles
Looking inside the /`app/`ui folder, there is a file called global.css. This file add CSS rules to all the routes in this application - such as CSS reset rules, site-wide styles for HTML elements like links, and more.

`global.css` will be imported in any component in this application, but it's usually good practice to add it to the top-level component. 

## Tailwind
Tailwind is a CSS framework that speeds up the development process by allowing you to quickly write utility classes directly in the TSX markup.

Although the CSS styles are shared globally, each class is singularly applied to each element. This means if you add or delete an element, you don't have to worry about maintaining separate stylesheets, style collisions, or the size of our CSS bundle growing as our application scales.


### CSS Modules
CSS Modules allow us to scope CSS to a component by automatically creating unique class names, so we don't have to worry about style collisions as well.

### Using the clsx library to toggle class names
There may be cases where we may need to conditionally style an element based on state or some other condition.

`clsx` is a library that lets us toggle class names easily. 


### Other styling solutions

- Sass which allows one to import .css and .scss files.
- CSS-in-JS libraries such as styled-jsx, styled-components, and emotion.



# Chapter 3
In this chapter, this is what we are treating;
- How to add custom fonts with `next/font`
- How to add images with `next/image`
- How fonts and images are optimized in Next.js

## Why optimize fonts?
Fonts play a significant role in the design of a website, but using custom fonts in our project can affect performance if the font files need to be fetched and loaded.

Cumulative Layout Shift is a metric used by Google to evaluate the performance and user experience of a website. With fonts, layout shift happens when the browser initially renders text in a fallback or system font and then swaps it out for a custom font once it has loaded. This swap can cause the text size, spacing, or layout to change, shifting elements around it.

Next.js automatically optimizes fonts in the application when you use the `next/font` module. It downloads font files at build time and hosts them with our other static assets. This means when a user visits our application, there are no additional network requests for fonts which would impact performance.


### Adding a primary font

In the `/`app/`ui` folder, a new file called `fonts.ts` is created. We will use this file to keep the fonts that will be used throughout the application.

We then import the `Inter` font from the `next/font/google` module - this will be our primary font. Then, specify what subset you'd like to load. In this case, `'latin'`:

Then we finally add the font to the `<body>` element in `/`app/`layout.tsx`:


## Why optimize images?
Next.js can serve static assets, like images, under the top-level /public folder. Files inside /public can be referenced in our application.

With regular HTML, that mean you have to manually: 
- Ensure our image is responsive on different screen sizes.
- Specify image sizes for different devices.
- Prevent layout shift as the images load.
- Lazy load images that are outside the user's viewport.

Image Optimization is a large topic in web development that could be considered a specialization in itself. Instead of manually implementing these optimizations, you can use the `next/image` component to automatically optimize our images.


## The `<Image>` component
The `<Image>` Component is an extension of the HTML `<img>` tag, and comes with automatic image optimization, such as:

- Preventing layout shift automatically when images are loading.
- Resizing images to avoid shipping large images to devices with a smaller viewport.
- Lazy loading images by default (images load as they enter the viewport).
- Serving images in modern formats, like WebP and AVIF, when the browser supports it.



# Chapter 4

## Creating Layouts and Pages

In this chapter, we'll cover 
- Create the dashboard routes using file-system routing.

- Understand the role of folders and files when creating new route segments.

- Create a nested layout that can be shared between multiple dashboard pages.

- Understand what colocation, partial rendering, and the root layout are.


## Nested routing
Next.js uses file-system routing where folders are used to create nested routes. Each folder represents a route segment that maps to a URL segment.

You can create separate UIs for each route using `layout.tsx `and `page.tsx` files.

`page.tsx` is a special Next.js file that exports a React component, and it's required for the route to be accessible. In this application, we already have a page file: `/`app/`page.tsx` - this is the home page associated with the route `/.`

To create a nested route, one can nest folders inside each other and add `page.tsx` files inside them. For example:

`/`app/`dashboard/page.tsx` is associated with the `/dashboard path`. 

By having a special name for page files, Next.js allows you to colocate UI components, test files, and other related code with our routes. Only the content inside the `page` file will be publicly accessible. For example, the `/ui` and `/lib` folders are colocated inside the `/app` folder along with our routes.


## Creating the dashboard layout

Dashboards have some sort of navigation that is shared across multiple pages. In Next.js, we can use a special layout.tsx file to create UI that is shared between multiple pages.

One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called partial rendering.


## Root Layout
In our `/`app/`layout.tsx`, we imported the `Inter` font. This is called a root layout and is required. Any UI you add to the root layout will be shared across all pages in our application. You can use the root layout to modify our `<html>` and `<body>` tags, and add metadata.



# Chapter 5

## Navigating Between Pages

In this chapter, we will cover

- How to use the next/link component.

- How to show an active link with the `usePathname()` hook.

- How navigation works in Next.js.

### Why optimize navigation?

To link between pages, you'd traditionally use the `<a>` HTML element. At the moment, the sidebar links use `<a>` elements, but notice what happens when you navigate between the home, invoices, and customers pages on our browser.

Did you see it?

There's a full page refresh on each page navigation!


## The `<Link>` component

In Next.js, you can use the <Link /> Component to link between pages in our application. <Link> allows you to do client-side navigation with JavaScript.

## Automatic code-splitting and prefetching
To improve the navigation experience, Next.js automatically code splits our application by route segments. This is different from a traditional React SPA, where the browser loads all our application code on initial load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

Furthermore, in production, whenever `<Link>` components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!


### How Routing and Navigation Works
The App Router uses a hybrid approach for routing and navigation. On the server, our application code is automatically code-split by route segments. And on the client, Next.js prefetches and caches the route segments. This means, when a user navigates to a new route, the browser doesn't reload the page, and only the route segments that change re-render - improving the navigation experience and performance.

1. Code Splitting
Code splitting allows you to split our application code into smaller bundles to be downloaded and executed by the browser. This reduces the amount of data transferred and execution time for each request, leading to improved performance.

Server Components allow our application code to be automatically code-split by route segments. This means only the code needed for the current route is loaded on navigation.

2. Prefetching
Prefetching is a way to preload a route in the background before the user visits it.

There are two ways routes are prefetched in Next.js:

- `<Link>` component: Routes are automatically prefetched as they become visible in the user's viewport. Prefetching happens when the page first loads or when it comes into view through scrolling.

- `router.prefetch()`: The `useRouter` hook can be used to prefetch routes programmatically.

The `<Link>`'s default prefetching behavior (i.e. when the `prefetch` prop is left unspecified or set to `null`) is different depending on our usage of `loading.js`. Only the shared layout, down the rendered "tree" of components until the first `loading.js` file, is prefetched and cached for `30s`. This reduces the cost of fetching an entire dynamic route, and it means you can show an instant loading state for better visual feedback to users.

You can disable prefetching by setting the `prefetch` prop to `false`. Alternatively, you can prefetch the full page data beyond the loading boundaries by setting the `prefetch` prop to `true`.


**Note**
- Prefetching is not enabled in development, only in production.

3. Caching
Next.js has an in-memory client-side cache called the Router Cache. As users navigate around the app, the React Server Component Payload of prefetched route segments and visited routes are stored in the cache.

This means on navigation, the cache is reused as much as possible, instead of making a new request to the server - improving performance by reducing the number of requests and data transferred.


4. Partial Rendering
Partial rendering means only the route segments that change on navigation re-render on the client, and any shared segments are preserved.

For example, when navigating between two sibling routes, /dashboard/settings and /dashboard/analytics, the settings and analytics pages will be rendered, and the shared dashboard layout will be preserved.

Without partial rendering, each navigation would cause the full page to re-render on the client. Rendering only the segment that changes reduces the amount of data transferred and execution time, leading to improved performance.

5. Soft Navigation
Browsers perform a "hard navigation" when navigating between pages. The Next.js App Router enables "soft navigation" between pages, ensuring only the route segments that have changed are re-rendered (partial rendering). This enables client React state to be preserved during navigation.

6. Back and Forward Navigation
By default, Next.js will maintain the scroll position for backwards and forwards navigation, and re-use route segments in the Router Cache.

7. Routing between `pages/` and `app/`
When incrementally migrating from `pages/` to `app/`, the Next.js router will automatically handle hard navigation between the two. To detect transitions from `pages/` to `app/`, there is a client router filter that leverages probabilistic checking of app routes, which can occasionally result in false positives. By default, such occurrences should be very rare, as we configure the false positive likelihood to be 0.01%. This likelihood can be customized via the `experimental.clientRouterFilterAllowedRate` option in `next.config.js`. It's important to note that lowering the false positive rate will increase the size of the generated filter in the client bundle.

Alternatively, if you prefer to disable this handling completely and manage the routing between `pages/` and `app/` manually, you can set `experimental.clientRouterFilter `to false in `next.config.js`. When this feature is disabled, any dynamic routes in pages that overlap with app routes won't be navigated to properly by default.


# Pattern: Showing active links
A common UI pattern is to show an active link to indicate to the user what page they are currently on. To do this, you need to get the user's current path from the URL. Next.js provides a hook called `usePathname()` that you can use to check the path and implement this pattern.

Since `usePathname()` is a hook, we will need to use the React's `"use client"` directive to the top of the file.



# Chapter 6

## Setting Up The Database

Before we can continue working on this dashboard, we'll need some data. In this chapter, we'll be setting up a PostgreSQL database using `@vercel/postgres`. If you're already familiar with PostgreSQL and would prefer to use our own provider, you can skip this chapter and set it up on our own. Otherwise, let's continue!

In this chapter, we’ll cover

- Push this project to GitHub.

- Set up a Vercel account and link our GitHub repo for instant previews and deployments.

- Create and link our project to a Postgres database.

- Seed the database with initial data.


## Create a Postgres database

After you have push this repository to GitHub and it is deployed, we now move to set up a database. Click Continue to Dashboard and select the Storage tab from our project dashboard. Select Connect Store → Create New → Postgres → Continue.

Accept the terms, assign a name to our database, and ensure our database region is set to Washington D.C (iad1) - this is also the default region for all new Vercel projects. By placing our database in the same region or close to our application code, you can reduce latency for data requests.

Once connected, navigate to the `.env`.local tab, click Show secret and Copy Snippet. Make sure you reveal the secrets before copying them.

Navigate to our code editor and rename the `.env.example` file to `.env`. Paste in the copied contents from Vercel.

Important: Go to our .gitignore file and make sure `.env` is in the ignored files to prevent our database secrets from being exposed when you push to GitHub.

Finally, run `npm i @vercel/postgres` in our terminal to install the Vercel Postgres SDK.


## Seed our database
Now that the  database has been created, let's seed it with some initial data.

In the /scripts folder of our project, there's a file called seed.js. This script contains the instructions for creating and seeding the invoices, customers, user, revenue tables. This script uses SQL to create the tables, and the data from placeholder-data.js file to populate them after they've been created.

Next, in our package.json file, add the following line to our scripts:

`"seed": "node -r dotenv/config ./scripts/seed.js"`

This is the command that will execute `seed.js`.

Now, run `npm run seed`. You should see some `console.log` messages in our terminal to let you know the script is running.



# Chapter 7

## Fetching Data

In this chapter, we’ll cover

- Learn about some approaches to fetching data: APIs, ORMs, SQL, etc.

- How Server Components can help you access back-end resources more securely.

- What network waterfalls are.

- How to implement parallel data fetching using a JavaScript Pattern.


## Choosing how to fetch data

### API layer

APIs are an intermediary layer between our application code and database. There are a few cases where you might use an API:

- If you're using 3rd party services that provide an API.
- If you're fetching data from the client, you want to have an API layer that runs on the server to avoid exposing our database secrets to the client.

In Next.js, you can create API endpoints using Route Handlers.

### Database queries

When you're creating a full-stack application, you'll also need to write logic to interact with our database. For relational databases like Postgres, you can do this with SQL, or an ORM like Prisma.

There are a few cases where you have to write database queries:

- When creating our API endpoints, you need to write logic to interact with our database.

- If you are using React Server Components (fetching data on the server), you can skip the API layer, and query our database directly without risking exposing our database secrets to the client.

### Using Server Components to fetch data
By default, Next.js applications use React Server Components. Fetching data with Server Components is a relatively new approach and there are a few benefits of using them:

- Server Components support promises, providing a simpler solution for asynchronous tasks like data fetching. You can use async/await syntax without reaching out for useEffect, useState or data fetching libraries.

- Server Components execute on the server, so you can keep expensive data fetches and logic on the server and only send the result to the client.

- As mentioned before, since Server Components execute on the server, you can query the database directly without an additional API layer.

### Using SQL
For this dashboard project, we'll write database queries using the Vercel Postgres SDK and SQL. There are a few reasons why we'll be using SQL:

- SQL is the industry standard for querying relational databases (e.g. ORMs generate SQL under the hood).

- Having a basic understanding of SQL can help you understand the fundamentals of relational databases, allowing you to apply our knowledge to other tools.

- SQL is versatile, allowing you to fetch and manipulate specific data.

- The Vercel Postgres SDK provides protection against SQL injections.


## Fetching data for the dashboard overview page

Now in our `/app/dashboard/page.tsx`, let's fetch data for the dashboard overview page.

However... there are two things you need to be aware of:

1. The data requests are unintentionally blocking each other, creating a request waterfall.

2. By default, Next.js prerenders routes to improve performance, this is called Static Rendering. So if our data changes, it won't be reflected in our dashboard.
Let's discuss number 1 in this chapter, then look into detail at number 2 in the next chapter.


## What are request waterfalls?
A "waterfall" refers to a sequence of network requests that depend on the completion of previous requests. In the case of data fetching, each request can only begin once the previous request has returned data.

For example, in our `page.tsx` we need to wait for `fetchRevenue()` to execute before `fetchLatestInvoices()` can start running, and so on.

This pattern is not necessarily bad. There may be cases where you want waterfalls because you want a condition to be satisfied before you make the next request. For example, you might want to fetch a user's ID and profile information first. Once you have the ID, you might then proceed to fetch their list of friends. In this case, each request is contingent on the data returned from the previous request.

However, this behavior can also be unintentional and impact performance.


## Parallel data fetching
A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel.

In JavaScript, you can use the `Promise.all()` or `Promise.allSettled()` functions to initiate all promises at the same time. For example, in data.ts, we're using `Promise.all()` in the `fetchCardData()` function.

By using this pattern, you can:

- Start executing all data fetches at the same time, which can lead to performance gains.
- Use a native JavaScript pattern that can be applied to any library or framework.

However, there is one disadvantage of relying only on this JavaScript pattern: **what happens if one data request is slower than all the others?**



# Chapter 8

## Static and Dynamic Rendering

In the previous chapter, you fetched data for the Dashboard Overview page. However, we briefly discussed two limitations of the current setup:

- The data requests are creating an unintentional waterfall.
- The dashboard is static, so any data updates will not be reflected on our application.

In this chapter, we’ll cover

- What static rendering is and how it can improve our application's performance.

- What dynamic rendering is and when to use it.

- Different approaches to make our dashboard dynamic.

- Simulate a slow data fetch to see what happens.


## What is Static Rendering?
With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or during revalidation. The result can then be distributed and cached in a Content Delivery Network (CDN).

Whenever a user visits our application, the cached result is served. There are a couple of benefits of static rendering:

- Faster Websites - Prerendered content can be cached and globally distributed. This ensures that users around the world can access our website's content more quickly and reliably.

- Reduced Server Load - Because the content is cached, our server does not have to dynamically generate content for each user request.

- SEO - Prerendered content is easier for search engine crawlers to index, as the content is already available when the page loads. This can lead to improved search engine rankings.

Static rendering is useful for UI with no data or data that is shared across users, such as a static blog post or a product page. It might not be a good fit for a dashboard that has personalized data that is regularly updated.

The opposite of static rendering is dynamic rendering.


## What is Dynamic Rendering?

With dynamic rendering, content is rendered on the server for each user at request time (when the user visits the page). There are a couple of benefits of dynamic rendering:

- Real-Time Data - Dynamic rendering allows our application to display real-time or frequently updated data. This is ideal for applications where data changes often.

- User-Specific Content - It's easier to serve personalized content, such as dashboards or user profiles, and update the data based on user interaction.

- Request Time Information - Dynamic rendering allows you to access information that can only be known at request time, such as cookies or the URL search parameters.


## Making the dashboard dynamic

By default, @vercel/postgres doesn't set its own caching semantics. This allows the framework to set its own static and dynamic behavior.

You can use a Next.js API called unstable_noStore inside our Server Components or data fetching functions to opt out of static rendering. Let's add this in our `data.ts`, import `unstable_noStore` from `next/cache`, and call it the top of our data fetching functions.

**Note:** unstable_noStore is an experimental API and may change in the future. If you prefer to use a stable API in our own projects, you can also use the Segment Config Option export const dynamic = "force-dynamic".


## Simulating a Slow Data Fetch
Making the dashboard dynamic is a good first step. However... there is still one problem we mentioned in the previous chapter. What happens if one data request is slower than all the others?

Let's simulate a slow data fetch. In our data.ts file, uncomment the console.log and `setTimeout` inside `fetchRevenue()`

Now open `http://localhost:3000/dashboard/ `in a new tab and notice how the page takes longer to load. In our terminal, you should also see the following messages:

`Fetching revenue data...`
`Data fetch completed after 3 seconds.`

Here, you've added an artificial 3-second delay to simulate a slow data fetch. The result is that now our whole page is blocked while the data is being fetched.

Which brings us to a common challenge developers have to solve:

With dynamic rendering, **our application is only as fast as our slowest data fetch**.



# Chapter 9

## Streaming
In the previous chapter, we made our dashboard page dynamic, however, we discussed how the slow data fetches can impact the performance of our application. Let's look at how we can improve the user experience when there are slow data requests.

In this chapter, we’ll cover

- What streaming is and when you might use it.

- How to implement streaming with `loading.tsx` and Suspense.

- What loading skeletons are.

- What route groups are, and when you might use them.

- Where to place Suspense boundaries in our application.


## What is streaming?
Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

By streaming, you can prevent slow data requests from blocking our whole page. This allows the user to see and interact with parts of the page without waiting for all the data to load before any UI can be shown to the user.

Streaming works well with React's component model, as each component can be considered a chunk.

There are two ways you implement streaming in Next.js:

1. At the page level, with the `loading.tsx` file.
2. For specific components, with <Suspense>.

Let's see how this works.

Streaming a whole page with `loading.tsx`
In the /app/dashboard folder, create a new file called `loading.tsx`.

A few things are happening here:

1. ``loading.tsx`` is a special Next.js file built on top of Suspense, it allows you to create fallback UI to show as a replacement while page content loads.

2. Since `<SideNav>` is static, it's shown immediately. The user can interact with `<SideNav>` while the dynamic content is loading.

3. The user doesn't have to wait for the page to finish loading before navigating away (this is called interruptable navigation).


## Adding loading skeletons

A loading skeleton is a simplified version of the UI. Many websites use them as a placeholder (or fallback) to indicate to users that the content is loading. Any UI we embed into `loading.tsx` will be embedded as part of the static file, and sent first. Then, the rest of the dynamic content will be streamed from the server to the client.


## Fixing the loading skeleton bug with route groups
Right now, our loading skeleton will apply to the invoices and customers pages as well.

Since `loading.tsx` is a level higher than `/invoices/page.tsx` and `/customers/page.tsx `in the file system, it's also applied to those pages.

We can change this with Route Groups. Create a new folder called `/(overview)` inside the dashboard folder. Then, move our `loading.tsx` and page.tsx files inside the folder:

Now, the `loading.tsx` file will only apply to our dashboard overview page.

Route groups allow us to organize files into logical groups without affecting the URL path structure. When we create a new folder using parentheses `()`, the name won't be included in the URL path. So `/dashboard/(overview)/page.tsx` becomes` /dashboard`.

Here, you're using a route group to ensure `loading.tsx` only applies to our dashboard overview page. However, you can also use route groups to separate our application into sections (e.g. `(marketing)` routes and `(shop)` routes) or by teams for larger applications.


## Streaming a component
So far, we're streaming a whole page. But, instead, we can be more granular and stream specific components using React Suspense.

Suspense allows us to defer rendering parts of our application until some condition is met (e.g. data is loaded). We can wrap our dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads.

If we remember the slow data request, `fetchRevenue()`, this is the request that is slowing down the whole page. Instead of blocking our page, you can use Suspense to stream only this component and immediately show the rest of the page's UI.


## Grouping components

We need to wrap the `<Card>` components in Suspense. We can fetch data for each individual card, but this could lead to a popping effect as the cards load in, this can be visually jarring for the user.

**So, how would you tackle this problem?**

To create more of a staggered effect, you can group the cards using a wrapper component. This means the static `<SideNav/> `will be shown first, followed by the cards, etc.


## Deciding where to place our Suspense boundaries
Where you place our Suspense boundaries will depend on a few things:

1. How you want the user to experience the page as it streams.

2. What content you want to prioritize.

3. If the components rely on data fetching.

4. Take a look at our dashboard page, is there anything you would've done differently?

*Don't worry. There isn't a right answer.*

- We could stream the whole page like we did with loading.tsx... but that may lead to a longer loading time if one of the components has a slow data fetch.

- We could stream every component individually... but that may lead to UI popping into the screen as it becomes ready.

- We  could also create a staggered effect by streaming page sections. But we'll need to create wrapper components.

Where we place our suspense boundaries will vary depending on our application. In general, it's good practice to move our data fetches down to the components that need it, and then wrap those components in Suspense. But there is nothing wrong with streaming the sections or the whole page if that's what our application needs.

Don't be afraid to experiment with Suspense and see what works best, it's a powerful API that can help us create more delightful user experiences.



# Chapter 10

## Partial Prerendering (Optional)
Partial Prerendering is an experimental feature introduced in Next.js 14. 

In this chapter, we'll topics we’ll cover

- What Partial Prerendering is.

- How Partial Prerendering works.


## Combining Static and Dynamic Content
Currently, if we call a dynamic function inside our route (e.g. `noStore()`, `cookies()`, etc), our entire route becomes dynamic.

This is how most web apps are built today. We either choose between static and dynamic rendering for our entire application or for a specific route.

However, most routes are not fully static or dynamic. We may have a route that has both static and dynamic content.


## What is Partial Prerendering?
Next.js 14 contains a preview of Partial Prerendering – an experimental feature that allows you to render a route with a static loading shell, while keeping some parts dynamic. In other words, you can isolate the dynamic parts of a route. For example:

When a user visits a route:

- A static route shell is served, ensuring a fast initial load.
- The shell leaves holes where dynamic content will load in asynchronous.
- The async holes are streamed in parallel, reducing the overall load time of the page.

This is different from how our application behaves today, where entire routes are either entirely static or dynamic.

Partial Prerendering combines ultra-quick static edge delivery with fully dynamic capabilities and we believe it has the potential to become the default rendering model for web applications, bringing together the best of static site generation and dynamic delivery.


## How does Partial Prerendering work?
Partial Prerendering leverages React's Concurrent APIs and uses Suspense to defer rendering parts of our application until some condition is met (e.g. data is loaded).

The fallback is embedded into the initial static file along with other static content. At build time (or during revalidation), the static parts of the route are prerendered, and the rest is postponed until the user requests the route.

It's worth noting that wrapping a component in Suspense doesn't make the component itself dynamic (remember you used unstable_noStore to achieve this behavior), but rather Suspense is used as a boundary between the static and dynamic parts of our route.

The great thing about Partial Prerendering is that you don't need to change our code to use it. As long as you're using Suspense to wrap the dynamic parts of our route, Next.js will know which parts of our route are static and which are dynamic.



# Summary
To recap, we've done a few things to optimize data fetching in our application, we've:

1. Created a database in the same region as our application code to reduce latency between our server and database.

2. Fetched data on the server with React Server Components. This allows us to keep expensive data fetches and logic on the server, reduces the client-side JavaScript bundle, and prevents our database secrets from being exposed to the client.

3. Used SQL to only fetch the data we needed, reducing the amount of data transferred for each request and the amount of JavaScript needed to transform the data in-memory.

4. Parallelize data fetching with JavaScript - where it made sense to do so.

5. Implemented Streaming to prevent slow data requests from blocking our whole page, and to allow the user to start interacting with the UI without waiting for everything to load.

6. Move data fetching down to the components that need it, thus isolating which parts of our routes should be dynamic in preparation for Partial Prerendering.



# Chapter 11

## Adding Search and Pagination
In this chapter, we’ll cover

- Learn how to use the Next.js APIs: searchParams, usePathname, and useRouter.

- Implement search and pagination using URL search params.

Let's spend some time familiarizing ourselves with the page and the components we'll be working with:

- `<Search/>` allows users to search for specific invoices.

- `<Pagination/>` allows users to navigate between pages of invoices.

- `<Table/>` displays the invoices.

This search functionality will span the client and the server. When a user searches for an invoice on the client, the URL params will be updated, data will be fetched on the server, and the table will re-render on the server with the new data.


## Why use URL search params?
We'll be using URL search params to manage the search state.

There are a couple of benefits of implementing search with URL params:

- **Bookmarkable and Shareable URLs:** Since the search parameters are in the URL, users can bookmark the current state of the application, including their search queries and filters, for future reference or sharing.

- **Server-Side Rendering and Initial Load:** URL parameters can be directly consumed on the server to render the initial state, making it easier to handle server rendering.

- **Analytics and Tracking:** Having search queries and filters directly in the URL makes it easier to track user behavior without requiring additional client-side logic.


## Adding the search functionality
These are the Next.js client hooks that we'll use to implement the search functionality:

- `useSearchParams` - Allows you to access the parameters of the current URL. For example, the search params for this URL `/dashboard/invoices?page=1&query=pending` would look like this: `{page: '1', query: 'pending'}`.

- `usePathname` - Lets you read the current URL's pathname. For example, for the route `/dashboard/invoices`, `usePathname` would return `'/dashboard/invoices'`.

- `useRouter` - Enables navigation between routes within client components programmatically.

Here's a quick overview of the implementation steps:

1. Capture the user's input.

2. Update the URL with the search params.

3. Keep the URL in sync with the input field.

4. Update the table to reflect the search query.


### `defaultValue` vs. `value` / Controlled vs. Uncontrolled

If you're using state to manage the `value` of an input, you'd use the value attribute to make it a controlled component. This means React would manage the input's state.

However, since you're not using state, you can use `defaultValue`. This means the native input will manage its own state. This is okay since you're saving the search query to the URL instead of state.


### When to use the useSearchParams() hook vs. the searchParams prop?

You might have noticed you used two different ways to extract search params. Whether you use one or the other depends on whether you're working on the client or the server.

- `<Search>` is a Client Component, so you used the `useSearchParams()` hook to access the params from the client.

- `<Table>` is a Server Component that fetches its own data, so you can pass the searchParams prop from the page to the component.

- As a general rule, if you want to read the params from the client, use the `useSearchParams()` hook as this avoids having to go back to the server.


### Here's a breakdown of what's happening in `/app/ui/search.tsx`:

- `${pathname}` is the current path, in your case, "/dashboard/invoices".

- As the user types into the search bar, `params.toString()` translates this input into a URL-friendly format.

- `replace(${pathname}?${params.toString()})` updates the URL with the user's search data. For example, `/dashboard/invoices?query=lee` if the user searches for "Lee".

- The URL is updated without reloading the page, thanks to Next.js's client-side navigation.


## Best practice: Debouncing

We're currently updating the URL on every keystroke, and therefore querying our database on every keystroke! This isn't a problem as our application is small, but imagine if our application had thousands of users, each sending a new request to the database on each keystroke.

Debouncing is a programming practice that limits the rate at which a function can fire. In our case, we only want to query the database when the user has stopped typing.

### How Debouncing Works:

1. **Trigger Event:** When an event that should be debounced (like a keystroke in the search box) occurs, a timer starts.

2. **Wait:** If a new event occurs before the timer expires, the timer is reset.

3. **Execution:** If the timer reaches the end of its countdown, the debounced function is executed.

We can implement debouncing in a few ways, including manually creating our own debounce function. To keep things simple, we'll use a library called `use-debounce`.

Install `use-debounce`:

 `npm i use-debounce`

By debouncing, you can reduce the number of requests sent to your database, thus saving resources.


## Adding pagination

After introducing the search feature, you'll notice the table displays only 6 invoices at a time. This is because the fetchFilteredInvoices() function in data.ts returns a maximum of 6 invoices per page.

Adding pagination allows users to navigate through the different pages to view all the invoices. Let's see how you can implement pagination using URL params, just like you did with search.

Navigate to the <Pagination/> component and you'll notice that it's a Client Component. You don't want to fetch data on the client as this would expose your database secrets (remember, you're not using an API layer). Instead, you can fetch the data on the server, and pass it to the component as a prop.

To summarize:

- We've handled search and pagination with URL search parameters instead of client state.

We've fetched data on the server.

We're using the useRouter router hook for smoother, client-side transitions.



# Chapter 12

## Mutating Data
Let's continue working on the Invoices page by adding the ability to create, update, and delete invoices!

In this chapter, we’ll cover

- What React Server Actions are and how to use them to mutate data.

- How to work with forms and Server Components.

- Best practices for working with the native formData object, including type validation.

- How to revalidate the client cache using the revalidatePath API.

- How to create dynamic route segments with specific IDs.

## What are Server Actions?
React Server Actions allow you to run asynchronous code directly on the server. They eliminate the need to create API endpoints to mutate your data. Instead, you write asynchronous functions that execute on the server and can be invoked from your Client or Server Components.

Security is a top priority for web applications, as they can be vulnerable to various threats. This is where Server Actions come in. They offer an effective security solution, protecting against different types of attacks, securing your data, and ensuring authorized access. Server Actions achieve this through techniques like POST requests, encrypted closures, strict input checks, error message hashing, and host restrictions, all working together to significantly enhance your app's safety.


## Using forms with Server Actions

In React, you can use the action attribute in the `<form>` element to invoke actions. The action will automatically receive the native FormData object, containing the captured data.

For example:

```nextjs
// Server Component
export default function Page() {
  // Action
  async function create(formData: FormData) {
    'use server';
 
    // Logic to mutate data...
  }
 
  // Invoke the action using the "action" attribute
  return <form action={create}>...</form>;
}
```

An advantage of invoking a Server Action within a Server Component is progressive enhancement - forms work even if JavaScript is disabled on the client.


## Next.js with Server Actions

Server Actions are also deeply integrated with Next.js caching. When a form is submitted through a Server Action, not only can you use the action to mutate data, but you can also revalidate the associated cache using APIs like `revalidatePath` and `revalidateTag`.


## Creating an invoice
Here are the steps we'll take to create a new invoice:

1. Create a form to capture the user's input.

2. Create a Server Action and invoke it from the form.

3. Inside our Server Action, extract the data from the formData object.

4. Validate and prepare the data to be inserted into the database.

5. Insert the data and handle any errors.

6. Revalidate the cache and redirect the user back to invoices page.
