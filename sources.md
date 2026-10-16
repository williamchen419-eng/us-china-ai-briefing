# Intelligence Sources

**Note:** this annotated list predates a later feed overhaul, so it no longer matches the feeds the prefetch job actually fetches. Its value is the owner / incentive / watch-for analysis of each outlet. Ownership notes are as of 2026; verify them before relying on them, and treat each "watch for" as a question to check, not a finding.

Each source is annotated with its owner, incentive structure, and what to watch for.

## Tier 1 — Primary (check every run)

### AI Model & Research
| Source | Owner/Incentive | Watch For |
|--------|----------------|-----------|
| [OpenAI Blog](https://openai.com/blog) | OpenAI — fundraising narrative, safety framing | Whether capability claims coincide with funding rounds |
| [Anthropic Blog](https://www.anthropic.com/research) | Anthropic — safety-first positioning, enterprise pitch | Whether research posts also serve product marketing |
| [Google AI Blog](https://blog.google/technology/ai/) | Alphabet — platform defense, cloud revenue | I/O announcements, Gemini positioning vs GPT |
| [Meta AI Blog](https://ai.meta.com/blog/) | Meta — open-source positioning, talent recruitment | Llama releases, open-weight strategy shifts |
| [DeepSeek](https://api-docs.deepseek.com/) | Chinese startup — efficiency narrative | Cost/performance claims, architecture innovations |

### Semiconductor & Compute
| Source | Owner/Incentive | Watch For |
|--------|----------------|-----------|
| [SemiAnalysis](https://www.semianalysis.com/) | Independent analyst — subscription revenue | Deep technical analysis, supply chain mapping |
| [Tom's Hardware](https://www.tomshardware.com/) | Consumer tech media — ad-supported | Chip benchmark leaks, manufacturing updates |
| [TSMC IR](https://investor.tsmc.com/) | TSMC — shareholder communication | Capex guidance, geographic capacity split |
| [NVIDIA Newsroom](https://nvidianews.nvidia.com/) | NVIDIA — demand narrative | Datacenter revenue, China export workarounds |

### Hyperscaler & Infrastructure
| Source | Owner/Incentive | Watch For |
|--------|----------------|-----------|
| [Google Cloud Blog](https://cloud.google.com/blog) | Google — cloud market share narrative | AI infrastructure investments, TPU updates |
| [AWS News Blog](https://aws.amazon.com/blogs/aws/) | Amazon — enterprise retention | Custom silicon (Trainium/Graviton), AI service launches |
| [Microsoft Azure Blog](https://azure.microsoft.com/en-us/blog/) | Microsoft — OpenAI partnership positioning | Copilot enterprise push, Azure AI capacity |

### China-Specific
| Source | Owner/Incentive | Watch For |
|--------|----------------|-----------|
| [SCMP Technology](https://www.scmp.com/tech) | SCMP (Alibaba-linked) — HK/international bridge | China tech policy, US-China framing from HK perspective |
| [CGTN](https://www.cgtn.com/) | **State media** — CCTV international arm | Official framing of China tech achievements, policy endorsements. Accessed via Google News RSS (`site:cgtn.com`). Read for the official framing aimed at international audiences. |
| [Xinhua English](https://www.xinhuanet.com/english/) | **State media** — CPC Central Propaganda Dept | Official narratives, policy priorities, what is emphasised vs. omitted. Accessed via Google News RSS (`site:english.news.cn`) — direct RSS feeds frozen since 2017. Framing is the signal — read through it. |
| [36Kr](https://36kr.com) | VC/startup ecosystem press — pro-market, tech-optimist | Chinese startup funding, product launches, industry sentiment from inside the ecosystem |
| [MIIT (工信部)](https://www.miit.gov.cn/) | Ministry of Industry & IT — **government** | AI policy directives, chip industry standards, "independent and controllable" (自主可控) signals |
| [MOST (科技部)](https://www.most.gov.cn/) | Ministry of Science & Technology — **government** | National R&D programs, AI lab approvals, science plans |
| [State Council (English)](https://english.www.gov.cn/) | Central government — **government** | High-level policy releases, five-year plan implementation. Accessed via Google News RSS (`site:english.www.gov.cn`). |
| [Nikkei Asia Tech](https://asia.nikkei.com/Business/Tech) | Nikkei — Japan-Asia business lens | Supply chain shifts, Japan-China semiconductor dynamics |
| [Caixin Technology](https://www.caixinglobal.com/technology/) | Caixin — commercial business press, comparatively independent reporting | Policy signals, SOE moves, funding rounds. Accessed via Google News RSS (`site:caixinglobal.com`). |
| [Reuters China Tech](https://www.reuters.com/technology/) | Reuters — wire service, speed-first | Breaking policy changes, sanctions, trade actions |

## Tier 2 — Secondary (check weekly or on trigger)

| Source | Owner/Incentive | Notes |
|--------|----------------|-------|
| [The Information](https://www.theinformation.com/) | Subscription business press | Paywall — check headlines and free previews only |
| [TechCrunch AI](https://techcrunch.com/category/artificial-intelligence/) | Yahoo Media — VC ecosystem coverage | Funding rounds, startup launches |
| [The Verge](https://www.theverge.com/ai-artificial-intelligence) | Vox Media — consumer tech angle | Product launches, policy reactions |
| [ArXiv cs.AI](https://arxiv.org/list/cs.AI/recent) | Academic preprints | Breakthrough papers, Chinese institution output |
| [Politico Tech](https://www.politico.com/technology) | Politico — DC policy lens | Export controls, CHIPS Act implementation |

## Tier 3 — Context (as needed)

| Source | Use Case |
|--------|----------|
| Company 10-K/10-Q filings | Verify capex claims against actual spending |
| Congressional hearing transcripts | Policy direction signals |
| Patent filings (Google Patents) | R&D direction indicators |
| GitHub trending (AI repos) | Open-source momentum shifts |

## Source Bias Framework

When analyzing any article, the agent should ask:
1. **Who owns this outlet?** (e.g., SCMP → Alibaba Group)
2. **What's the business model?** (ads, subscriptions, IR)
3. **Who benefits from this framing?** (the company profiled, a competitor, a government)
4. **Is this new information or repackaged?** (press release → not news)
5. **What's NOT being said?** (omissions often matter more than inclusions)
