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
Before you start this project, make sure your system meets the following requirements:

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

Open `http://localhost:3000` on your browser. 


# Chapter 2

## CSS Styling
- How to add a global CSS file to your application.

- Two different ways of styling: Tailwind and CSS modules.

- How to conditionally add class names with the clsx utility package.

### Global Styles
Looking inside the /`app/`ui folder, there is a file called global.css. This file add CSS rules to all the routes in this application - such as CSS reset rules, site-wide styles for HTML elements like links, and more.

`global.css` will be imported in any component in this application, but it's usually good practice to add it to the top-level component. 

## Tailwind
Tailwind is a CSS framework that speeds up the development process by allowing you to quickly write utility classes directly in the TSX markup.

Although the CSS styles are shared globally, each class is singularly applied to each element. This means if you add or delete an element, you don't have to worry about maintaining separate stylesheets, style collisions, or the size of your CSS bundle growing as your application scales.


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
Fonts play a significant role in the design of a website, but using custom fonts in your project can affect performance if the font files need to be fetched and loaded.

Cumulative Layout Shift is a metric used by Google to evaluate the performance and user experience of a website. With fonts, layout shift happens when the browser initially renders text in a fallback or system font and then swaps it out for a custom font once it has loaded. This swap can cause the text size, spacing, or layout to change, shifting elements around it.

Next.js automatically optimizes fonts in the application when you use the `next/font` module. It downloads font files at build time and hosts them with your other static assets. This means when a user visits your application, there are no additional network requests for fonts which would impact performance.


### Adding a primary font

In the `/`app/`ui` folder, a new file called `fonts.ts` is created. We will use this file to keep the fonts that will be used throughout the application.

We then import the `Inter` font from the `next/font/google` module - this will be our primary font. Then, specify what subset you'd like to load. In this case, `'latin'`:

Then we finally add the font to the `<body>` element in `/`app/`layout.tsx`:


## Why optimize images?
Next.js can serve static assets, like images, under the top-level /public folder. Files inside /public can be referenced in your application.

With regular HTML, that mean you have to manually: 
- Ensure your image is responsive on different screen sizes.
- Specify image sizes for different devices.
- Prevent layout shift as the images load.
- Lazy load images that are outside the user's viewport.

Image Optimization is a large topic in web development that could be considered a specialization in itself. Instead of manually implementing these optimizations, you can use the `next/image` component to automatically optimize your images.


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

By having a special name for page files, Next.js allows you to colocate UI components, test files, and other related code with your routes. Only the content inside the `page` file will be publicly accessible. For example, the `/ui` and `/lib` folders are colocated inside the `/app` folder along with your routes.


## Creating the dashboard layout

Dashboards have some sort of navigation that is shared across multiple pages. In Next.js, we can use a special layout.tsx file to create UI that is shared between multiple pages.

One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called partial rendering.


## Root Layout
In our `/`app/`layout.tsx`, we imported the `Inter` font. This is called a root layout and is required. Any UI you add to the root layout will be shared across all pages in your application. You can use the root layout to modify your <html> and <body> tags, and add metadata.



# Chapter 5

## Navigating Between Pages

In this chapter, we will cover

- How to use the next/link component.

- How to show an active link with the `usePathname()` hook.

- How navigation works in Next.js.

### Why optimize navigation?

To link between pages, you'd traditionally use the <a> HTML element. At the moment, the sidebar links use <a> elements, but notice what happens when you navigate between the home, invoices, and customers pages on your browser.

Did you see it?

There's a full page refresh on each page navigation!


## The `<Link>` component

In Next.js, you can use the <Link /> Component to link between pages in your application. <Link> allows you to do client-side navigation with JavaScript.

## Automatic code-splitting and prefetching
To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

Furthermore, in production, whenever `<Link>` components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!


### How Routing and Navigation Works
The App Router uses a hybrid approach for routing and navigation. On the server, your application code is automatically code-split by route segments. And on the client, Next.js prefetches and caches the route segments. This means, when a user navigates to a new route, the browser doesn't reload the page, and only the route segments that change re-render - improving the navigation experience and performance.

1. Code Splitting
Code splitting allows you to split your application code into smaller bundles to be downloaded and executed by the browser. This reduces the amount of data transferred and execution time for each request, leading to improved performance.

Server Components allow your application code to be automatically code-split by route segments. This means only the code needed for the current route is loaded on navigation.

2. Prefetching
Prefetching is a way to preload a route in the background before the user visits it.

There are two ways routes are prefetched in Next.js:

- `<Link>` component: Routes are automatically prefetched as they become visible in the user's viewport. Prefetching happens when the page first loads or when it comes into view through scrolling.

- `router.prefetch()`: The `useRouter` hook can be used to prefetch routes programmatically.

The `<Link>`'s default prefetching behavior (i.e. when the `prefetch` prop is left unspecified or set to `null`) is different depending on your usage of `loading.js`. Only the shared layout, down the rendered "tree" of components until the first `loading.js` file, is prefetched and cached for `30s`. This reduces the cost of fetching an entire dynamic route, and it means you can show an instant loading state for better visual feedback to users.

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

Before we can continue working on this dashboard, we'll need some data. In this chapter, we'll be setting up a PostgreSQL database using `@vercel/postgres`. If you're already familiar with PostgreSQL and would prefer to use your own provider, you can skip this chapter and set it up on your own. Otherwise, let's continue!

In this chapter, we’ll cover

- Push this project to GitHub.

- Set up a Vercel account and link your GitHub repo for instant previews and deployments.

- Create and link your project to a Postgres database.

- Seed the database with initial data.
