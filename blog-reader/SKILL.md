---
name: blog-reader
description: >
  读取并讲解网页博客 / 帖子。当用户发来一个网址（任意博客文章 / Medium / 公众号 /
  Substack / 个人技术博客，也包括 X / Twitter 的推文或长文）并希望「总结 / 读这篇 /
  讲讲 / 教我这篇 / 带我学 / 讲解」时使用。优先用轻量抓取（firecrawl-scrape / WebFetch）
  读正文；遇到登录墙 / 付费墙 / 重 JS 的 SPA（如 X，裸抓会 402 / 空壳）再升级到浏览器兜底
  （需登录态优先走可用的真实 Chrome 连接器，如 Codex Chrome / Claude in Chrome；公开重 JS 页走 playwright CLI）。读到正文后**直接生成一个中文 HTML 讲解页并在浏览器打开**
  给用户看；讲解页默认保留，用户想清理时再删。默认不在聊天里长篇大论、不来回追问。
  Trigger when the user sends a blog / post / article URL and wants it summarized,
  read, explained, or taught. Falls back to a real logged-in browser for pages that
  plain fetching cannot read.
user-invocable: true
compatibility: >
  依赖 html-explainer skill 生成讲解页（不可用时自己写自包含 HTML 兜底）；
  生成讲解页前若有 frontend-design skill 就先加载它，让页面更精致、不像通用 AI 模板。
  抓取按需用到：firecrawl-scrape skill / WebFetch（轻量）、Codex Chrome / Claude in Chrome
  （带登录态）、playwright CLI（headless，需 node）。这些都是按需降级，缺哪个就走下一档。
---

# blog-reader — 读博客 / 帖子 → 直接出 HTML 讲解页

把用户给的网址读出正文，**直接生成一个中文 HTML 讲解页并打开**给用户看，看完再清理。

## 核心原则
- **不需要对话**：不要先在聊天里给总结、也不要问「要不要深入」。读完就直接出 HTML 并打开。
- 讲解页默认保留，不在最后追问是否删除；用户明确要求清理时再删除本次生成的文件。
- 聊天里只留极简信息（生成了什么 + 路径），正文都进 HTML。

## 何时用
- 用户给了一个 URL（博客、Medium、Substack、公众号、个人/技术博客、X/Twitter 推文或 thread 等），
  并表达「总结 / 读这篇 / 讲讲 / 教我 / 带我学 / 讲解」之类的意图。
- 只给 URL、没说意图 → 也按本流程走（默认就是出 HTML 讲解页）。
- 明显的非阅读类浏览器操作（下单、填表、登录等）**不属于**本 skill。

## 输入解析
- 提取消息里的 URL。
- 多个 URL：先问用户先读哪个，或是否依次各出一个 HTML；不要默默只读第一个。
- 没有 URL：提示用户贴一个链接。

## 第 1 步 · 读取页面（分层降级，按需升级）

**默认走轻量抓取，只有在真的读不到时才升级到浏览器。** 别一上来就开浏览器——大多数公开博客
轻量抓取就能秒读，开浏览器既慢又多依赖。

### 1a. 判断起点
- **已知需要登录态 / 付费墙 / 重 JS 的站点**（典型：`x.com` / `twitter.com`，部分付费 Medium、
  需登录的内网文档）→ 直接跳到 **1c 浏览器**，不必先试轻量抓取（多半会 402 / 空壳，浪费一轮）。
- **其它一律先走 1b 轻量抓取。**

### 1b. 轻量抓取（首选）
- 优先用 **firecrawl-scrape** skill（能渲染 JS 的 SPA、返回干净 markdown，比裸 WebFetch 稳）。
- 没有 firecrawl 时退到 **WebFetch**。
- 抓到后做一次**质量自检**，命中任一条就判定「读不全」，升级到 1c：
  - 正文几乎为空 / 只有导航壳 / 满是「请登录 / Subscribe to read」之类的墙提示；
  - 内容明显被截断、是长 thread 但只拿到开头；
  - 返回 401 / 402 / 403 或反爬拦截页。
- 读全了 → 直接进 **第 2 步**。

### 1c. 浏览器兜底（按需选）

轻量抓取读不全时升级到真实浏览器渲染。**先判断是否需要登录态，再选择当前环境可用的浏览器能力**：

| 选哪条 | 适用 | 关键差异 |
|--------|------|---------|
| **A. 真实 Chrome 连接器** | 内容需要**登录态**（X 登录后才显示全、用户已订阅的付费文、内网文档） | 复用用户真实浏览器会话，带 cookie；可用实现包括 Codex Chrome / Claude in Chrome |
| **B. playwright CLI** | **公开但重 JS** 的页面、firecrawl 没渲染好、或 Chrome 连接器不可用 | headless、无需扩展、纯命令行；但**无登录态** |

> 经验法则：需要登录才能看全 → 走 A；只是 JS 渲染问题、内容本身公开 → 走 B（更省事，不依赖扩展）。
> A 不可用就退到 B，B 也读不到（多半是真需要登录）再如实告知用户。

#### A. 真实 Chrome 连接器（带登录态）
按当前会话中可用的连接器选择实现，不要把流程绑定死在某一套工具名上：

1. **Codex Chrome**：如果可用技能列表里有 `Chrome:control-chrome` 或用户显式提到 `@Chrome`，
   先加载并遵循该 skill。用它连接用户的 Chrome 会话，新建或复用空白标签页，导航到目标 URL，
   等待渲染，滚动展开，再从页面正文提取文本。该插件的具体 API 以它自己的文档为准；不要臆造工具名。
2. **Claude in Chrome**：如果当前会话提供 Claude in Chrome 连接器，就按该扩展的实际文档连接浏览器、
   选择标签页、导航、等待、读取页面文本。不要假设具体 MCP 工具名；以已暴露工具为准。
3. **通用原则**：
   - 优先新建或复用空白标签页，不要覆盖用户正在看的页面。
   - X / Twitter、Substack、Medium 等 SPA 必须等待渲染；正文被截断、长 thread、有「Show more / 展开」时，
     滚动或点击展开后再次读取。
   - 连接工具偶发失败时，重新选择 / 重连一次；仍不可用就转 **B. playwright**，不要硬等。
   - 真正需要登录而 Chrome 连接器不可用时，如实告诉用户需要先在 Chrome 中登录或连接浏览器扩展。

#### B. playwright CLI（headless，无需扩展）
用一个小脚本渲染页面、取 `article`/`body` 正文。需要 node + playwright（首次 `npx playwright` 会自动装包，
浏览器内核首次用 `npx playwright install chromium` 装一次）。

把下面脚本写到**系统临时目录**里（不要落在工作目录，免得留垃圾）：
macOS / Linux 写到 `$TMPDIR/scrape.mjs`（或 `/tmp/scrape.mjs`），Windows 写到 `$env:TEMP\scrape.mjs`，
然后 `node "<该路径>" "<url>"`：

```js
import { chromium } from 'playwright';
const url = process.argv[2];
const browser = await chromium.launch();           // headless
const page = await browser.newPage();
await page.goto(url, { waitUntil: 'networkidle', timeout: 30000 });
await page.waitForTimeout(2000);                   // SPA 再缓一下
// 长 thread / 懒加载：滚到底触发加载
for (let i = 0; i < 8; i++) { await page.mouse.wheel(0, 4000); await page.waitForTimeout(500); }
const text = await page.evaluate(() =>
  (document.querySelector('article') ?? document.body).innerText);
console.log(text);
await browser.close();
```

- 读到正文 → 进 **第 2 步**；读到的还是墙提示/空壳 → 多半真需登录，转 A 或如实告知。
- 用完**删掉这个临时脚本**（`rm`/`Remove-Item`），别留在磁盘上。

### 兜底都不行时
- A、B 都读不到 → 如实告诉用户「只读到 X，正文需要登录/付费才能看全；可连接当前环境支持的 Chrome
  插件 / 浏览器扩展后我再用带登录态的方式读」，然后停下等待。
- **登录墙 / 付费墙 / 空白**：如实说明已读到什么、还差什么，**绝不脑补正文**。

## 第 2 步 · 直接生成 HTML 讲解页（调用 html-explainer）

读到正文后，**立刻**生成一个自包含、可在浏览器打开的**中文**讲解页：
- **先加载 `frontend-design` skill（若可用）**，套用它的排版 / 配色 / 动效 / 版式准则，让讲解页有设计感、不像通用 AI 模板。
- 先按下述规则确定**完整的目标文件路径**，再把该路径交给 `html-explainer` skill 生成页面；`html-explainer` 应优先使用调用方提供的路径，不应用自己的全局默认目录（其内部也会优先加载 frontend-design）。
> 若 `html-explainer` 不可用，就自己写一个自包含 HTML（内联 CSS、无外部依赖、可双击打开）兜底，内容清单照旧；这种兜底情况下同样先加载 frontend-design、套用其审美准则。

保存到固定目录（按平台取用户文档目录）：

- macOS / Linux：`~/Documents/blog-reader/<slug>.html`
- Windows：`%USERPROFILE%\Documents\blog-reader\<slug>.html`

`<slug>` 用文章主题的简短英文/拼音 kebab 串（如 `agentic-engineering-hacks`）。先确保目录存在：

```bash
# macOS / Linux
mkdir -p ~/Documents/blog-reader
```
```powershell
# Windows (PowerShell)
New-Item -ItemType Directory -Force "$env:USERPROFILE\Documents\blog-reader" | Out-Null
```

生成前检查完整目标路径是否已存在。除非用户明确要求覆盖，否则在文件名后追加下一个可用的递增编号，例如 `<slug>-2.html`、`<slug>-3.html`。路径优先级、项目目录判断和最终产物生命周期等通用规则遵循 `html-explainer`；本工作流明确用上述 `blog-reader` 目录覆盖其全局默认目录。

HTML 里应包含（中文）：
- **标题 + 出处**：作者 / 平台 / 时间（识别不到就略，别编）+ 原文链接。
- **一句话主旨**。
- **核心要点**：分点，抓真观点。
- **知识点讲解**：对值得学的点，按「是什么 / 为什么这么做 / 怎么用（步骤或最小示例）/ 常见坑」展开——这是「带我学」的核心，要讲透，不是只罗列。
- **关键结论 / 可操作信息**。
- **关键图 / 图表**：正文里承载信息的架构图、流程图、代码截图等，用 `<img src="原图URL">` 直接引用原图（不下载到本地），并配一句中文说明它在讲什么。识别不到图就略。
- **简短点评**：可信度、立场/偏向、是否带推广性质、可借鉴 vs 需谨慎。

## 第 3 步 · 打开给用户看

用 `Bash` 打开生成的 HTML（用默认浏览器）：
- macOS：`open "<绝对路径>"`
- Windows：`Start-Process "<绝对路径>"`（PowerShell）
- Linux：`xdg-open "<绝对路径>"`

然后在聊天里只回一两句：讲解页已生成并打开 + 文件路径。**不要**把讲解内容再在聊天里复述一遍。

## 第 4 步 · 收尾（默认保留，删除按需）

讲解页本身是有价值的学习材料，**默认保留**——尤其「教我 / 带我学」场景，用户多半想留着反复看。
所以不要每次都追着问「要不要删」。收尾只需：
- 给出文件路径，并轻量带一句：**「看完想清理的话告诉我，我删掉就行。」**
- 用户明确说要删 → 删除**这一个文件**（只删本次生成的，不动其他），删完确认一句。
  - macOS / Linux：`rm "<该 html 路径>"`；Windows：`Remove-Item "<该 html 路径>"`。
- 用户没提删除 / 不确定 → 保留，结束。

## 诚实原则（重要）
- 读不全就说读不全，分清「已读到的」和「没读到的」，不靠常识/训练记忆补正文。
- 引用作者观点标注来源；区分「原文事实」与「我的点评」。

## 语言
- HTML 讲解页、聊天回复一律**中文**。
- 工具调用、命令、代码、URL、文件名保持英文 / 原样。
