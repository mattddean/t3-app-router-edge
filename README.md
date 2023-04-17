# Archived

This repository has been superceeded by [a repo](https://github.com/mattddean/t3-app-router-edge-drizzle) that replaces Prisma and Kysely with Drizzle ORM.

>It's okay to use Prisma with Kysely, but having separate schema management and querying complicates things a bit. For example, if you add the `@updatedAt` flag to a column in Prisma, Prisma relies on its runtime to update the column rather than the database, but when querying with Kysely, Kysely will not automatically provide the a value for that column. But if you use Drizzle for schema management, specifying `.onUpdateNow()` on a column will cause the database to update this column for you on each update.

# T3 App Router (Edge)

An experimental attempt at using the fantastic T3 Stack entirely on the Edge runtime, with Next.js's beta App Router.

This is meant to be a place of hacking and learning. We're still learning how to structure apps using Next.js's new App Router, and comments are welcome in Discussions.

If you encounter an error (you will), please create an Issue so that we can fix bugs and learn together.

**This is not intended for production.** For a production-ready full-stack application, use much more most stable [create-t3-app](https://github.com/t3-oss/create-t3-app).

This project is not affiliated with create-t3-app.

## Features

This project represents the copy-pasting of work and ideas from a lot of really smart people. I think it's useful to see them all together in a working prototype.

- Edge runtime for all pages and routes.
- Type-safe SQL with Kysely (plus Prisma schema management)
  - While create-t3-app uses Prisma, Prisma can't run on the Edge runtime.
- Type-safe API with tRPC
  - App Router setup is copied from [here](https://github.com/trpc/next-13).
- Owned Authentication with Auth.js
  - Kysely adapter is copied from [here](https://github.com/nextauthjs/next-auth/pull/5464).
  - create-t3-app uses NextAuth, which doesn't support the Edge runtime. This project uses NextAuth's successor, Auth.js, which does. Since Auth.js hasn't built support for Next.js yet, their [SolidStart implementation](https://github.com/nextauthjs/next-auth/tree/36ad964cf9aec4561dd4850c0f42b7889aa9a7db/packages/frameworks-solid-start/src) is copied and slightly modified.
- Styling with [Tailwind](https://tailwindcss.com/)
  - It's just CSS, so it works just fine in the App Router.
- React components and layout from [shadcn/ui](https://github.com/shadcn/ui)
  - They're also just CSS and Radix, so they work just fine in the App Router.

## Data Fetching

There are a few options that Server Components + tRPC + React Query afford us. The flexibility of these tools allows us to use different strategies for different cases on the same project.

1. Fetch data on the server and render on the server or pass it to client components. [Example.](https://github.com/mattddean/t3-app-router-edge/blob/03cd3c0d16fb08a208279e08d90014e8e4fc8322/src/app/profile/page.tsx#L14)
1. Fetch data on the server and use it to hydrate react-query's cache on the client. Example: [Fetch and dehydrate data on server](https://github.com/mattddean/t3-app-router-edge/blob/c64d8dd8246491b7c4314c764b13d493b616df09/src/app/page.tsx#L19-L39), then [use cached data from server on client](https://github.com/mattddean/t3-app-router-edge/blob/03cd3c0d16fb08a208279e08d90014e8e4fc8322/src/components/posts-table.tsx#L84-L87).
1. Fetch data on the client.
1. Fetch data the server but don't block first byte and stream Server Components to the client using a Suspense boundary. TODO: Example.
