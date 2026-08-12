# SEO in Next.js 2026: Best Approach

For a **Next.js app in 2026**, the best SEO solution depends on whether you need a metadata manager, a helper library, or advanced structured data support.

For modern Next.js applications using the **App Router**, the recommended approach is to use the **built-in Next.js Metadata API** instead of installing a large SEO package.

---

## 🥇 Best Overall: Next.js Built-in Metadata API

For Next.js 13+ App Router projects (including Next.js 16), you usually **do not need an SEO module**.

Next.js provides built-in support for:

- Static metadata
- Dynamic metadata
- Open Graph tags
- Twitter cards
- Robots configuration
- Sitemap generation

Common files:

```text
app/
├── layout.tsx
├── robots.ts
└── sitemap.ts
```

---

## Basic Metadata Example

```ts
// app/layout.tsx

import type { Metadata } from "next";

export const metadata: Metadata = {
  title: {
    default: "My Kanban App",
    template: "%s | My Kanban App",
  },
  description: "A collaborative kanban board application",
  keywords: ["kanban", "tasks", "productivity"],
  openGraph: {
    title: "My Kanban App",
    description: "Manage your projects easily",
    type: "website",
  },
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return children;
}
```

---

## Dynamic Metadata

For dynamic routes, use `generateMetadata()`.

Example:

```ts
export async function generateMetadata({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;

  const board = await getBoard(slug);

  return {
    title: board.name,
    description: board.description,
  };
}
```

Example structure:

```text
app/
└── boards/
    └── [slug]/
        └── page.tsx
```

---

# 🥈 SEO Helper Library: next-seo

next-seo

`next-seo` is useful when you need:

- Reusable SEO components
- Easier JSON-LD handling
- Predefined schema helpers
- Less repetitive metadata code

Example:

```tsx
<NextSeo
  title="My Page"
  description="Page description"
  openGraph={{
    url: "https://example.com",
    title: "My Page",
    description: "Description",
  }}
/>
```

However, for App Router projects, it is usually unnecessary because Next.js already provides most SEO functionality.

---

# 🥉 Structured Data: schema-dts

schema-dts

For advanced SEO features such as:

- Google rich results
- Article schemas
- Product schemas
- Software application schemas

`schema-dts` provides TypeScript support for Schema.org data.

Example:

```ts
const jsonLd = {
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  name: "Kanban Board",
};
```

Then add it to the page:

```tsx
<script
  type="application/ld+json"
  dangerouslySetInnerHTML={{
    __html: JSON.stringify(jsonLd),
  }}
/>
```

---

# Recommended Setup for Next.js 16 + TypeScript

For a modern Next.js App Router project:

```text
app/
├── layout.tsx          # Global metadata
├── sitemap.ts          # Sitemap generation
├── robots.ts           # Search engine rules
└── boards/
    └── [slug]/
        └── page.tsx    # Dynamic metadata
```

Optional helper:

```text
lib/
└── seo.ts              # Reusable metadata functions
```

Example:

```ts
export function createMetadata({
  title,
  description,
}: {
  title: string;
  description: string;
}) {
  return {
    title,
    description,
    openGraph: {
      title,
      description,
    },
  };
}
```

---

# What to Avoid

Avoid older SEO approaches designed for the Pages Router:

❌ `next/head`

❌ Manually editing `<head>` tags

❌ Large SEO packages for simple applications

These approaches are mostly outdated for modern App Router projects.

---

# Recommendation for a Next.js 16 Kanban Application

For a Kanban SaaS-style application:

- Use **Next.js Metadata API** as the main SEO solution
- Add `sitemap.ts` and `robots.ts`
- Use `generateMetadata()` for public dynamic pages
- Add `schema-dts` only if you need rich search results later
- Avoid installing SEO packages unless the project grows significantly

**Recommended stack:**

```text
Next.js Metadata API
        +
robots.ts
        +
sitemap.ts
        +
(optional) schema-dts
```

This keeps the project lightweight, follows current Next.js standards, and avoids unnecessary dependencies.
