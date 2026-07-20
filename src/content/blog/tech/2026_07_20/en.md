---
title: "CDN (Content Delivery Network)"
pubDate: 2026-06-23
description: Basic View about the CDN
draft: false
slugId: tech/260623
---

# What Is a CDN? A Practical Guide (with Provider Comparison)

A **CDN (Content Delivery Network)** is a geographically distributed network of servers that caches content close to users. Instead of every request traveling all the way back to your origin server, the CDN serves a cached copy from an **edge server** near the user.

> **Analogy:** Think of your origin server as a central warehouse and the edge servers as local convenience stores. Rather than everyone driving across the country to the warehouse, they grab what they need from the nearest store.

If you want the official explanations, each major provider has a solid write-up:

- [Cloudflare — What is a CDN?](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/)
- [AWS — What is a CDN?](https://aws.amazon.com/what-is/cdn/)
- [Akamai — What is a CDN?](https://www.akamai.com/glossary/what-is-a-cdn)
- [Fastly — What is a CDN?](https://www.fastly.com/learning/cdn/what-is-a-cdn)
- [Imperva — What is a CDN & How It Works](https://www.imperva.com/learn/performance/what-is-cdn-how-it-works/)
- [IBM — Content Delivery Networks](https://www.ibm.com/think/topics/content-delivery-networks)

---

## Why Use a CDN? (The Problems It Solves)

A CDN is not just "make the site faster." It attacks four distinct problems:

- **Lower latency.** Content travels a shorter physical distance, so round-trip time (RTT) drops. A user in Tokyo hits a Tokyo edge instead of a server in Virginia.
- **Reduced origin load.** Every cache _hit_ never reaches your origin. Your backend handles a fraction of the traffic, which means fewer servers and less scaling pressure.
- **Bandwidth cost savings.** Offloading traffic to the edge reduces expensive origin egress. For media-heavy sites this is often the biggest line-item saving.
- **Availability & resilience.** The edge can keep serving cached content even when the origin is down, and it absorbs traffic spikes (flash sales, viral posts) that would otherwise overwhelm the backend.
- **Security.** Most CDNs double as a security layer: DDoS mitigation, a Web Application Firewall (WAF), bot management, and TLS termination at the edge.

---

## How a CDN Works

The whole system rests on a few core concepts:

- **Edge servers / PoPs (Points of Presence)** — the cache nodes spread across the globe.
- **Cache hit vs. cache miss** — a _hit_ is served from the edge; a _miss_ forces a trip to the origin.
- **Cache key, TTL, and `Cache-Control`** — what identifies a cached object, and how long it stays valid.
- **Cache invalidation / purge** — how you force the edge to drop stale content when you ship an update.

The request lifecycle for a typical pull-based CDN:

1. A user requests an asset (e.g. `/logo.png`).
2. The request is routed to the **nearest edge** (via Anycast IP or DNS — see below).
3. The edge checks its cache for that key.
4. **Cache hit** → serve immediately. **Cache miss** → fetch from the origin, store a copy, then serve it.
5. Every subsequent request within the TTL is served straight from the edge — no origin trip.

The single most important metric here is **cache hit ratio**: the percentage of requests served from the edge. A high hit ratio is the entire point of a CDN.

---

## Different Implementation Approaches

CDNs aren't all built the same way. There are three axes worth understanding.

### 1. Pull CDN vs. Push CDN

| Approach     | How it works                                                                                                         | Best for                                                                                                             |
| ------------ | -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Pull CDN** | You don't pre-upload anything. The edge _pulls_ from the origin on the first cache miss, then caches it per the TTL. | Sites with frequently changing content; the default for Cloudflare and CloudFront. Easiest to set up.                |
| **Push CDN** | You proactively _upload_ content into the CDN's storage and control exactly what's stored and when.                  | Large static files, infrequently changing assets, or low-traffic sites where you want to avoid origin hits entirely. |

### 2. Request Routing: Anycast vs. DNS-based

- **Anycast** — the same IP address is announced from many locations, and BGP routes the user to the nearest PoP automatically. Used by Cloudflare and Fastly. Simple, with fast failover.
- **DNS-based (GeoDNS)** — the DNS resolver hands back a different edge IP depending on the user's location. This is the traditional Akamai-style approach and gives very fine-grained routing control.

### 3. Static Caching vs. Edge Compute

- **Traditional CDN** caches _static_ assets — images, CSS, JS, video, fonts.
- **Edge compute platforms** let you run _code_ at the edge to personalize or transform responses near the user. Examples: Cloudflare Workers, AWS Lambda@Edge / CloudFront Functions, Fastly Compute, and Vercel Edge Functions. This blurs the line between "CDN" and "application platform" — you can do auth, A/B testing, or even render pages at the edge.

---

## Real-World Case Studies: How Top Companies Cache

Theory clicks faster next to real systems. These three cases climb a ladder — from **building your own CDN**, to **a provider's internals**, to **caching baked into an application framework**. The same vocabulary (hit ratio, push vs. pull, tiered caching, stale-while-revalidate) reappears at every level.

### Netflix — Open Connect: a custom CDN with proactive caching

Netflix didn't rent a CDN; it built one. **Open Connect** places custom servers called OCAs (Open Connect Appliances) _inside_ ISP data centers, so video travels the shortest possible hop to the viewer. Crucially, it's a **push** model: popular titles are pre-loaded onto thousands of OCAs during off-peak hours, so the content is already there before anyone hits play. The payoff is that roughly **95% of traffic is served straight from the edge** — never touching Netflix's AWS backend — at cache hit rates near **98%**.

The engineering goes all the way down to the OS: zero-copy delivery (FreeBSD's `sendfile()`) ships bytes from disk to NIC without bouncing through user space, and TLS encryption is offloaded to the network card. For live streams, Netflix fronts its origin with a Memcached-based write-through cache (EVCache) to survive "origin storms" when many edges fill at once.

- Netflix TechBlog — [https://netflixtechblog.com](https://netflixtechblog.com/)
- Open Connect — [https://openconnect.netflix.com](https://openconnect.netflix.com/)

### Cloudflare — Pingora + Tiered Cache: inside a CDN provider

Cloudflare openly documents how its own cache works, which is rare and worth reading. Two pieces stand out:

- **Pingora**, a Rust proxy built to replace NGINX. It handles over a trillion requests a day using roughly a third of the CPU and memory, largely by sharing connections across threads — one large customer's origin connection reuse jumped from ~87% to ~99.9%.
- **Tiered Cache**, which splits data centers into _lower tiers_ (near users) and _upper tiers_ (near origins). A lower-tier miss asks an upper tier before anyone goes to origin, raising hit ratio and collapsing origin connections. On top of this, **Cache Reserve** parks long-tail content in R2 object storage for 30 days, and the new cache layer does asynchronous **stale-while-revalidate** — serving stale content instantly while refreshing in the background.
- How we built Pingora — [https://blog.cloudflare.com/how-we-built-pingora-the-proxy-that-connects-cloudflare-to-the-internet/](https://blog.cloudflare.com/how-we-built-pingora-the-proxy-that-connects-cloudflare-to-the-internet/)
- Tiered Cache docs — [https://developers.cloudflare.com/cache/how-to/tiered-cache/](https://developers.cloudflare.com/cache/how-to/tiered-cache/)
- Cache Reserve — [https://blog.cloudflare.com/cache-reserve-open-beta/](https://blog.cloudflare.com/cache-reserve-open-beta/)

### Vercel / Next.js — ISR: caching at the application layer

**Incremental Static Regeneration (ISR)** pushes CDN-style caching up into the framework — the stale-while-revalidate pattern made first-class. A visitor gets the cached page instantly while Vercel regenerates it in the background, either on a timer or on demand. Two invalidation strategies are worth comparing:

- **Time-based** (`revalidate: 3600`) — dead simple, but content can be stale for up to the interval.
- **On-demand** (`revalidateTag` / `revalidatePath`) — tag your data, then fire a webhook from your CMS to invalidate exactly the affected pages. Near-instant and atomic, at the cost of wiring up a trigger.

Because Vercel knows which paths are cacheable _before_ the first request, it can also do request collapsing, ~300 ms global purges, and instant rollbacks. You can watch it work via the `x-nextjs-cache` header (`HIT` / `STALE` / `MISS` / `REVALIDATED`).

- Vercel ISR docs — [https://vercel.com/docs/incremental-static-regeneration](https://vercel.com/docs/incremental-static-regeneration)
- Next.js ISR guide — [https://nextjs.org/docs/app/guides/incremental-static-regeneration](https://nextjs.org/docs/app/guides/incremental-static-regeneration)

> **The pattern across all three:** push vs. pull, a high cache hit ratio, tiered/origin-shielding topologies, and stale-while-revalidate keep reappearing. Learn them once and you'll spot them everywhere — including in system-design interviews.

---

## CDN Provider Comparison

|Provider|Type / Approach|Core Strength|Typical Use Case|Free Tier|Website|
|---|---|---|---|---|---|
|**Cloudflare**|Full edge platform (CDN + DNS + security + edge compute)|Easy setup, strong security, global Anycast network|Startups, SaaS, general web apps|Generous free plan|[cloudflare.com](https://www.cloudflare.com/)|
|**AWS CloudFront**|AWS-native CDN, integrated with the AWS ecosystem|Deep AWS integration (S3, EC2, Lambda)|Enterprise AWS architectures|Limited free tier|[aws.amazon.com/cloudfront](https://aws.amazon.com/cloudfront/)|
|**Fastly**|Real-time edge CDN|Near-instant cache invalidation + API-driven control|High-frequency dynamic content, APIs|Trial credit|[fastly.com](https://www.fastly.com/)|
|**Akamai**|Enterprise-grade global CDN|Largest, most mature global network|Banks, governments, Fortune 500|Sales-led (no self-serve free tier)|[akamai.com](https://www.akamai.com/)|
|**Vercel**|Frontend-optimized CDN (built into the platform)|Zero-config, optimized for React / Next.js|Frontend apps, SSR/SSG sites|Free Hobby tier|[vercel.com](https://vercel.com/)|

---

## How to Choose

A quick decision guide:

- **Building a general web app or SaaS and want one tool for CDN + DNS + security?** → Cloudflare.
- **Already all-in on AWS?** → CloudFront, for the native S3/Lambda integration.
- **Need real-time, programmatic control over caching for APIs or dynamic content?** → Fastly.
- **A large enterprise with strict compliance and global scale needs?** → Akamai.
- **Shipping a Next.js / React frontend?** → Vercel, for the zero-config experience.

For most side projects and startups, **Cloudflare's free tier is the pragmatic starting point** — you get a real CDN, DNS, and basic DDoS protection without touching a credit card, and you can grow into edge compute later.

---

_Key takeaways: a CDN cuts latency by serving from the edge, shields your origin from load, and increasingly does double duty as a security and edge-compute layer. The metric that matters most is your cache hit ratio._