---
title: "CDN(内容分发网络)"
pubDate: 2026-03-23
description: 关于 CDN 的基础认识
draft: false
slugId: tech/260623
---

# 什么是 CDN?一份实用指南(附主流厂商对比)

**CDN(Content Delivery Network,内容分发网络)** 是一个在地理上分布式部署的服务器网络,它把内容缓存到离用户更近的地方。这样请求就不必每次都长途跋涉回到你的源站(origin server),而是由离用户最近的**边缘服务器(edge server)**直接返回一份缓存副本。

> **类比:** 把源站想象成一个中央仓库,把边缘服务器想象成遍布各地的便利店。与其让所有人都开车横穿全国去仓库,不如就近在便利店拿到想要的东西。

如果你想看各家官方的解释,几个主流厂商都有不错的文章:

- [Cloudflare — What is a CDN?](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/)
- [AWS — What is a CDN?](https://aws.amazon.com/what-is/cdn/)
- [Akamai — What is a CDN?](https://www.akamai.com/glossary/what-is-a-cdn)
- [Fastly — What is a CDN?](https://www.fastly.com/learning/cdn/what-is-a-cdn)
- [Imperva — What is a CDN & How It Works](https://www.imperva.com/learn/performance/what-is-cdn-how-it-works/)
- [IBM — Content Delivery Networks](https://www.ibm.com/think/topics/content-delivery-networks)

---

## 为什么要用 CDN?(它解决了哪些问题)

CDN 不只是"让网站更快"这么简单。它针对的是几类截然不同的问题:

- **降低延迟(latency)。** 内容传输的物理距离更短,往返时间(RTT)随之下降。东京的用户命中的是东京的边缘节点,而不是远在弗吉尼亚的服务器。
- **减轻源站负载。** 每一次缓存命中(cache _hit_)都不会到达源站。你的后端只需处理一小部分流量,这意味着更少的服务器和更小的扩容压力。
- **节省带宽成本。** 把流量卸载到边缘,能减少昂贵的源站出口流量(egress)。对图片/视频密集的站点,这往往是省钱最大的一项。
- **可用性与韧性。** 即使源站宕机,边缘仍能继续提供缓存内容;它还能吸收突发流量(秒杀、爆款帖子),避免这些冲击压垮后端。
- **安全。** 大多数 CDN 同时充当一层安全防护:DDoS 缓解、Web 应用防火墙(WAF)、bot 管理,以及在边缘完成 TLS 终止。

---

## CDN 是怎么工作的

整个系统建立在几个核心概念之上:

- **边缘服务器 / PoP(Points of Presence,接入点)** —— 遍布全球的缓存节点。
- **缓存命中 vs. 缓存未命中(cache hit vs. cache miss)** —— 命中由边缘直接返回;未命中则必须回源一趟。
- **Cache key、TTL 与 `Cache-Control`** —— 分别决定如何标识一个缓存对象,以及它能保持多久有效。
- **缓存失效 / 清除(cache invalidation / purge)** —— 当你发布更新时,如何强制边缘丢弃过期内容。

典型 pull 型 CDN 的请求生命周期:

1. 用户请求某个资源(例如 `/logo.png`)。
2. 请求被路由到**最近的边缘节点**(通过 Anycast IP 或 DNS —— 见下文)。
3. 边缘节点按 cache key 检查自己的缓存。
4. **命中** → 立即返回。**未命中** → 回源拉取,存一份副本,再返回。
5. 在 TTL 有效期内的后续请求,全部由边缘直接返回 —— 不再回源。

这里最重要的一个指标是**缓存命中率(cache hit ratio)**:由边缘提供的请求占比。高命中率正是 CDN 存在的全部意义。

---

## 不同的实现方式

CDN 的构建方式并不都一样。有三个值得理解的维度。

### 1. Pull CDN vs. Push CDN

| 方式         | 工作原理                                                                                     | 适用场景                                                                       |
| ------------ | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **Pull CDN** | 你不预先上传任何东西。边缘在第一次缓存未命中时从源站**拉取(pull)**,之后按 TTL 缓存。       | 内容频繁变化的站点;Cloudflare 和 CloudFront 的默认方式。最易上手。             |
| **Push CDN** | 你主动把内容**上传(push)**进 CDN 的存储,精确控制存什么、何时存。                          | 大型静态文件、不常变动的资源,或想彻底避免首次回源的低流量站点。               |

### 2. 请求路由:Anycast vs. DNS-based

- **Anycast** —— 同一个 IP 地址从多个位置对外宣告,BGP 自动把用户路由到最近的 PoP。Cloudflare 和 Fastly 用的就是这种。简单,且故障切换快。
- **DNS-based(GeoDNS)** —— DNS 解析器根据用户位置返回不同的边缘 IP。这是 Akamai 式的传统做法,能提供非常细粒度的路由控制。

### 3. 静态缓存 vs. 边缘计算(Edge Compute)

- **传统 CDN** 缓存的是**静态**资源 —— 图片、CSS、JS、视频、字体。
- **边缘计算平台** 让你在边缘运行**代码**,在离用户很近的地方个性化或改写响应。例如:Cloudflare Workers、AWS Lambda@Edge / CloudFront Functions、Fastly Compute、Vercel Edge Functions。这模糊了"CDN"和"应用平台"的界线 —— 你可以在边缘做鉴权、A/B 测试,甚至直接渲染页面。

---

## 实战案例:头部公司是怎么做缓存的

理论放在真实系统旁边会更好理解。下面三个案例层层递进 —— 从**自建 CDN**,到**CDN 厂商的内部机制**,再到**烤进应用框架里的缓存**。同一套词汇(命中率、push vs. pull、分层缓存、stale-while-revalidate)在每一层反复出现。

### Netflix —— Open Connect:自建 CDN + 主动预热缓存

Netflix 没有租用 CDN,而是自己造了一个。**Open Connect** 把叫做 OCA(Open Connect Appliance)的定制服务器直接放进 ISP 机房**内部**,让视频以最短的跳数到达观众。关键在于它是 **push** 模型:热门内容会在低峰时段提前预加载到成千上万台 OCA 上,等用户点击播放前内容就已经在那了。结果是大约 **95% 的流量直接由边缘提供** —— 完全不碰 Netflix 的 AWS 后端 —— 缓存命中率接近 **98%**。

工程优化一路深入到操作系统层面:zero-copy 传输(FreeBSD 的 `sendfile()`)让数据从磁盘直送网卡,不必在用户态来回拷贝;TLS 加密则被卸载到网卡上完成。对于直播流,Netflix 在源站前面加了一层基于 Memcached 的 write-through 缓存(EVCache),以扛住大量边缘同时回源填充时的"origin storm(回源风暴)"。

- Netflix TechBlog — [https://netflixtechblog.com](https://netflixtechblog.com/)
- Open Connect — [https://openconnect.netflix.com](https://openconnect.netflix.com/)

### Cloudflare —— Pingora + Tiered Cache:CDN 厂商的内部视角

Cloudflare 公开记录了自家缓存的工作原理,这很难得,值得一读。两块尤其突出:

- **Pingora**,一个用 Rust 写来替代 NGINX 的代理。它每天处理超过一万亿次请求,却只用了原来约三分之一的 CPU 和内存,主要靠跨线程共享连接 —— 某个大客户的源站连接复用率从约 87% 提升到约 99.9%。

- **Tiered Cache(分层缓存)**,把数据中心分成**下层(lower tier,离用户近)**和**上层(upper tier,离源站近)**。下层未命中时会先问上层,而后才有人回源,从而提高命中率、收敛源站连接。在此之上,**Cache Reserve** 把长尾内容存进 R2 对象存储达 30 天;新的缓存层还实现了异步 **stale-while-revalidate** —— 立即返回旧内容,同时在后台刷新。

- How we built Pingora — [https://blog.cloudflare.com/how-we-built-pingora-the-proxy-that-connects-cloudflare-to-the-internet/](https://blog.cloudflare.com/how-we-built-pingora-the-proxy-that-connects-cloudflare-to-the-internet/)

- Tiered Cache docs — [https://developers.cloudflare.com/cache/how-to/tiered-cache/](https://developers.cloudflare.com/cache/how-to/tiered-cache/)

- Cache Reserve — [https://blog.cloudflare.com/cache-reserve-open-beta/](https://blog.cloudflare.com/cache-reserve-open-beta/)

### Vercel / Next.js —— ISR:应用层的缓存

**增量静态再生(Incremental Static Regeneration,ISR)** 把 CDN 式的缓存上移进了框架 —— 把 stale-while-revalidate 模式做成了一等公民。访客立刻拿到缓存页面,与此同时 Vercel 在后台重新生成它,触发方式可以是定时,也可以是按需。两种失效策略值得对比:

- **基于时间**(`revalidate: 3600`)—— 极其简单,但内容在一个间隔内可能是 stale 的。
- **按需**(`revalidateTag` / `revalidatePath`)—— 给数据打 tag,然后从 CMS 发一个 webhook,精确地让受影响的页面失效。近乎实时且整页原子替换,代价是要搭一套触发机制。

因为 Vercel 在第一个请求到来**之前**就知道哪些路径可缓存,它还能做请求合并(request collapsing)、约 300ms 的全球 purge,以及即时回滚。你可以通过 `x-nextjs-cache` 响应头(`HIT` / `STALE` / `MISS` / `REVALIDATED`)观察它的行为。

- Vercel ISR docs — [https://vercel.com/docs/incremental-static-regeneration](https://vercel.com/docs/incremental-static-regeneration)
- Next.js ISR guide — [https://nextjs.org/docs/app/guides/incremental-static-regeneration](https://nextjs.org/docs/app/guides/incremental-static-regeneration)

> **三个案例共通的模式:** push vs. pull、高缓存命中率、分层 / 源站屏蔽(origin-shielding)的拓扑,以及 stale-while-revalidate,会一再出现。学会一次,你就能在任何地方认出它们 —— 包括系统设计面试。

---

## CDN 厂商对比

| 厂商             | 类型 / 定位                                  | 核心优势                                  | 典型场景                       | 免费额度                 | 官网                                                      |
| ---------------- | -------------------------------------------- | ----------------------------------------- | ------------------------------ | ------------------------ | --------------------------------------------------------- |
| **Cloudflare**   | 全功能边缘平台(CDN + DNS + 安全 + 边缘计算) | 上手简单、安全能力强、全球 Anycast 网络   | 创业公司、SaaS、通用 Web 应用  | 慷慨的免费版             | [cloudflare.com](https://www.cloudflare.com/)             |
| **AWS CloudFront** | AWS 原生 CDN,深度集成 AWS 生态             | 与 AWS 深度集成(S3、EC2、Lambda)        | 企业级 AWS 架构                | 有限免费额度             | [aws.amazon.com/cloudfront](https://aws.amazon.com/cloudfront/) |
| **Fastly**       | 实时边缘 CDN                                 | 近乎即时的缓存失效 + API 驱动的控制        | 高频动态内容、API              | 试用额度                 | [fastly.com](https://www.fastly.com/)                     |
| **Akamai**       | 企业级全球 CDN                               | 规模最大、最成熟的全球网络                | 银行、政府、世界 500 强        | 销售对接(无自助免费版) | [akamai.com](https://www.akamai.com/)                     |
| **Vercel**       | 前端优化型 CDN(平台内置)                   | 零配置,为 React / Next.js 优化            | 前端应用、SSR/SSG 站点         | 免费 Hobby 版            | [vercel.com](https://vercel.com/)                         |

---

## 如何选择

一份快速决策指南:

- **想用一个工具同时搞定 CDN + DNS + 安全,做通用 Web 应用或 SaaS?** → Cloudflare。
- **已经全面押注 AWS?** → CloudFront,图它和 S3/Lambda 的原生集成。
- **需要对 API 或动态内容做实时、可编程的缓存控制?** → Fastly。
- **大型企业,有严格合规和全球规模需求?** → Akamai。
- **要上线 Next.js / React 前端?** → Vercel,图它的零配置体验。

对大多数个人项目和创业公司来说,**Cloudflare 的免费版是最务实的起点** —— 不用绑信用卡就能拿到一个真正的 CDN、DNS 和基础 DDoS 防护,之后再慢慢用上边缘计算。

---

_核心要点:CDN 通过在边缘提供内容来降低延迟,替源站挡住负载,并且越来越多地兼任安全与边缘计算这两重角色。而最重要的指标,始终是你的缓存命中率(cache hit ratio)。_