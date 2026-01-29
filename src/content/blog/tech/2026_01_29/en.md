---
title: "Building My First Personal Blog: Astro + GitHub Pages in Practice"
pubDate: 2026-01-29
description: A record of building my first personal blog using Astro and GitHub Pages
draft: false
slugId: tech/260129
---

## 1. Motivation

I have always wanted to build a personal blog to record my learning notes, thoughts, and daily reflections.  
In the past, I did write some blog posts, but they were all published on third-party platforms. While those platforms are convenient and stable, I always felt that a personal blog was less “secure” compared to them.

However, with the rapid development of AI, I’ve become increasingly aware of one thing:  
**personal uniqueness is becoming more and more important.**

If you want to truly accumulate and present that uniqueness over the long term, a platform that you fully control is clearly a better choice. A personal blog website fits this idea very well.

So I spent some time building my own blog.  
This article is the very first post on the site, and also a **complete record of the entire setup process**. I’ve tried to write it in as much detail as possible, so that even **non-professionals** can follow along and build their own blog step by step.

---

## 2. Tech Stack Selection

Before getting started, I’d like to briefly explain how I chose the blogging framework.

I had previously built a blog using **Hexo**, but when I tried it again this time, I ran into several issues:

- Many themes are quite old, mostly from seven or eight years ago  
- I was using a relatively new version of Node.js locally  
- A lot of themes and plugins were no longer compatible, which increased the maintenance cost  

So I decided to ask AI for recommendations on **modern tools suitable for building a personal website today**.

My core requirements were actually very simple:

- Can be deployed to **GitHub Pages**  
- No need to pay for a server  
- Modern tech stack with low maintenance cost  

After comparing several options, I finally chose **Astro**.

🔗 Astro official site: https://astro.build/

---

### Comparison of Popular Static Site Generators

| Framework | Tech Stack | Performance / Build | Modern Architecture | Interactive Components | Learning Curve | Use Cases |
| --- | --- | --- | --- | --- | --- | --- |
| **Astro** | JS / TS + multi-framework | Fast build & runtime, zero JS by default | ⭐⭐⭐⭐ (Islands Architecture) | ⭐⭐⭐ (on-demand loading) | ⭐⭐ | Content-driven blogs, docs |
| **Hugo** | Go | Extremely fast builds | ⭐⭐ | ⭐ | ⭐⭐ | Content-heavy blogs |
| **Hexo** | Node.js | Fast builds | ⭐⭐ | ⭐ (plugin-based) | ⭐⭐ | Lightweight blogs |
| **Jekyll** | Ruby | Medium | ⭐⭐ | ⭐ | ⭐ | Native GitHub Pages support |

The reason I ultimately chose Astro is simple:  
**it’s modern, high-performance, blog-friendly, and very mature when it comes to GitHub Pages deployment.**

---

## 3. Setup Steps

### 1️⃣ Finding and Choosing a Theme

Astro provides an official theme marketplace:

🔗 Astro Themes: https://astro.build/themes

> **Step 1**  
> Visit Astro Themes and choose a theme you like (both free and paid options are available).

![](./20260129132628.png "")

> **Step 2**  
> On the theme detail page:
> - **Get Started** links to the GitHub repository  
> - **Live Demo** lets you preview the blog online  

![](./20260129132652.png "")

> **Step 3**  
> Once you’re on the GitHub repository page, you can simply `git clone` the project to your local machine.  
> This step requires some basic Git knowledge. If you’re not familiar with Git, it’s recommended to look up the basics first.

![](./20260129133122.png "")

---

### 2️⃣ Running the Project Locally

> **Step 4**  
> Most theme repositories include a `README.md` file explaining:
> - Required environment  
> - How to install dependencies  
> - How to start the local development server  

Make sure your local environment is properly set up, then follow the author’s instructions exactly.

![](./20260129133545.png "")

> **Step 5**  
> After the project starts successfully, the terminal will usually provide a local URL (for example, `http://localhost:3000`).  
> Open it in your browser and verify that the site loads correctly.

![](./20260129133705.png "")

---

### 3️⃣ Deploying to GitHub Pages

Official documentation:

- GitHub Pages: https://docs.github.com/en/pages/quickstart  
- GitHub Actions: https://docs.github.com/en/actions  

> **Step 6**  
> Create a new repository on GitHub.  
> ⚠️ Note: the repository name must follow GitHub Pages naming rules. Please refer to the official documentation for details.

> **Step 7**  
> Push your local code to the repository:
> - Update the `git remote` URL  
> - Make sure the code is successfully pushed  

Then enable **GitHub Pages** in the repository settings and configure the build source.

![](./20260129134529.png "")

> **Step 8**  
> Set up GitHub Actions for automatic deployment.  
> You can follow Astro’s official deployment guide here:  
> https://www.astrojs.cn/en/guides/deploy/github/

Once the GitHub Action build succeeds, you’ll be able to access your blog via the generated URL.

![](./20260129134338.png "")

---

## 4. Conclusion

At this point, a personal blog built with **Astro + GitHub Pages** is fully up and running 🎉  

If you’re also considering building your own blog, I hope this article can be helpful to you.
