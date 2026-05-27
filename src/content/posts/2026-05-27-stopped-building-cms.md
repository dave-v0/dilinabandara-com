---
title: "Why I stopped building a CMS for my own blog"
date: 2026-05-27
description: "On the particular madness of building infrastructure for writing instead of just writing."
---

There is a specific kind of procrastination available only to people who can build software. It looks like work. It produces artefacts — schemas, endpoints, admin panels — that feel satisfying to point at. It has its own vocabulary of productivity: I'm setting up the foundation, getting the infrastructure right, making sure I won't have to redo this later.

I spent three weeks building a CMS for this blog before I wrote a single post.

## The logic, as it seemed at the time

The reasoning was coherent, as this kind of reasoning always is. I wanted to write from anywhere. I wanted drafts that auto-saved. I wanted to be able to schedule posts, add tags, manage a small reading list alongside the essays. A flat-file system felt too simple — what if I wanted to do more later? What if I needed to query across posts?

> Software built for a future self is almost always software built for an imaginary person who has different problems than you do.

So I built the CMS. I used a database. I wrote a little rich-text editor integration. I built an auth system, because of course you need auth on your personal admin panel. I wrote a deployment pipeline. I thought very seriously about image optimization.

At some point I realised I had built a moderately complex web application and was no closer to having anything to say.

## What I was actually doing

The CMS was a way of not starting. This is not a new observation — plenty of people have said it better — but it's one of those things you have to discover personally before you believe it. Understanding it abstractly doesn't protect you from doing it.

What made it seductive was that the CMS had clear success conditions. A form submits. A record appears in the database. An endpoint returns `200`. Writing has no such checkpoints. A paragraph either says what you mean or it doesn't, and you're often the last person to know which.

The technical work was also genuinely interesting, in the way that technical work tends to be. I was making real decisions about real tradeoffs:

- Should drafts be stored as HTML or Markdown? (I chose Markdown.)
- Should the admin be separate from the public site or same-origin? (Separate, then I reversed it.)
- How do you handle image uploads on a statically-deployed site? (I never finished solving this one.)

None of these decisions mattered, because I had no posts.

## The actual solution

I deleted the CMS. I set up Astro with Markdown files in a `src/content/posts/` directory. The entire configuration is a few dozen lines. The deployment is a GitHub Actions workflow that runs `npm run build` and pushes to Pages.

```bash
npm create astro@latest -- --template minimal
```

The content schema is a handful of Zod fields:

```typescript
schema: z.object({
  title: z.string(),
  date: z.date(),
  description: z.string().optional(),
  draft: z.boolean().optional().default(false),
})
```

I write posts in my editor. I commit them. They appear. There is nothing to log into, no database to back up, no session to manage.

The constraint — no admin panel, no GUI, just text files — turned out to be a feature. The friction of opening a markdown file and writing something is low enough that I actually do it. The friction of building and maintaining a CMS was, evidently, high enough that I wrote zero posts over three weeks while doing it.

Software-as-procrastination is most dangerous when it's indistinguishable from software-as-progress. The cure is not discipline, exactly. It's choosing tools that make the real work easier rather than tools that make the meta-work more interesting.

Markdown files in a git repo are not exciting. That's precisely the point.
