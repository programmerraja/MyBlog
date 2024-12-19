---
title: NextJs Notes
date: 2024-11-30T06:52:07.077+05:30
draft: true
tags:
  - programming
---

`npx create-next-app@latest my-app`


## Folder structure

- app	        App Router
- pages	Pages Router
- public	Static assets to be served
- src	        Optional application source folder

Private folders can be created by prefixing a folder with an underscore: `_folderName`

## Routers

Next.js has two different routers: the App Router (nextjs 13) and the Pages Router. The App Router is a newer router that allows you to use React's latest features, such as Server Components and Streaming. The Pages Router is the original Next.js router, which allowed you to build server-rendered React applications

  
Nested route

A nested route is a route composed of multiple URL segments. For example, the `/blog/[slug]` route is composed of three segments:
- `/` (Root Segment)
- `blog` (Segment)
- `[slug]` (Leaf Segment)
### APP Router 

In the `app` directory, nested folders define route structure. Each folder represents a route segment that is mapped to a corresponding segment in a URL path.

- **File-based routing** in the `app` directory.
- **Nested Layouts**: Define layouts per folder for reusability.
- **Server Components**: Pages are server-rendered by default.
- **React Suspense and Streaming**: Allows partial rendering and progressive hydration.
- **Loading, Error, and Not Found Handlers**: Built-in components for handling these states.

However, even though route structure is defined through folders, a route is **not publicly accessible** until a `page.js` or `route.js` file is added to a route segment.

So if we want we can create a component under app dir but it will not be considered as route
```
- app
  - button
	- button.tsx
```

**Types of routes**

**Dynamic Routes** 

Dynamic Segments are passed as the `params` prop to component

`const slug = (await params).slug`

```
/app
  ├── (product)
  │    ├── [slug]/
  │         ├── page.js   (/product/:slug)

```
 
**Route Groups** in the **App Router** allow you to organize routes and layouts without affecting the URL structure.

when we wrap a folder with () it wont be consider for URL it will avalibel under / path 

```
/app
  ├── (groupA)
  │    ├── layout.js        # Layout for Group A
  │    ├── page.js          # Page under Group A (/)
  │    ├── about/
  │         ├── page.js     # Page under Group A (/about)
  ├── (groupB)
  │    ├── layout.js        # Layout for Group B
  │    ├── page.js          # Page under Group B (/)
  │    ├── contact/
  │         ├── page.js     # Page under Group B (/contact)

```
## Page Router

Page Routes are part of the **Pages Directory** and were the default routing system before Next.js 13. It's simpler but less flexible compared to the App Router.

1. **File-based routing** in the `pages` directory.
2. **No layouts** (handled manually via `_app.js`).
3. **Client Components**: Everything is client-rendered by default unless explicitly server-rendered.
4. **Basic API routes**: API endpoints are created in the `pages/api` folder.

Navigation
- Link 
- useRouter

backend routes
- file name with routes.js

## layouts

A layout is UI that is **shared** between multiple pages. On navigation, layouts preserve state, remain interactive, and do not rerender.

You can define a layout by default exporting a React component from a layout file. The component should accept a `children` prop which can be a page or another layout.

For example, to create a layout that accepts your index page as child, add a `layout` file inside the `app` directory:

```r
app
 - layout.ts
 - page.ts

export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        {/* Layout UI */}
        {/* Place children where you want to render a page or nested layout */}
        <main>{children}</main>
      </body>
    </html>
  )
}
```
-  The root layout is **required** and must contain `html` and `body` tags.

we can create layout for each specfic page like  create a layout for the `/blog` route, add a new `layout` file inside the `blog` folder.
```
blog
 - layout.ts
 - page.ts
```



## Rendering

#### Server 

By default, Next.js uses Server Components.

On the server, Next.js uses React's APIs to orchestrate rendering. The rendering work is split into chunks: by individual route segments and [Suspense Boundaries](https://react.dev/reference/react/Suspense)

Each chunk is rendered in two steps:

1. React renders Server Components into a special data format called the **React Server Component Payload (RSC Payload)**.
2. Next.js uses the RSC Payload and Client Component JavaScript instructions to render **HTML** on the server.

Then, on the client:

1. The HTML is used to immediately show a fast non-interactive preview of the route - this is for the initial page load only.
2. The React Server Components Payload is used to reconcile the Client and Server Component trees, and update the DOM.
3. The JavaScript instructions are used to [hydrate](https://react.dev/reference/react-dom/client/hydrateRoot) Client Components and make the application interactive.

 Three subsets of server rendering: 
 - Static -> routes are rendered at **build time**
 - Dynamic ->  routes are rendered for each user at **request time**.
 - Streaming.


#### Client

`"use client"` is used to declare a boundary  between a Server and Client Component modules. This means that by defining a `"use client"` in a file, all other modules imported into it, including child components, are considered part of the client bundle.









Library
- https://next-auth.js.org/ 