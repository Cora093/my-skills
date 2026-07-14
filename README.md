# my-skills

> 各处收集或自己提炼的，我觉得好用的 Claude Code / Agent **skill 收藏镜像**。
> 不一定是原创。

---

```
my-skills/
├── README.md           # 本文件(含收藏索引表)
├── .gitignore
├── <skill-a>/          # 收藏的 skill,根目录平铺
│   └── SKILL.md
├── <skill-b>/
│   ├── SKILL.md
│   └── ...(辅助文件)
└── ...
```

## 收藏索引

| 技能名 | 一句话用途 | 添加时间 |
|--------|-----------|---------|
| teach-me | 陪练式拆解一次会话/改动,边讲边测直到你真懂 | 2026-06-04 |
| html-explainer | 把复杂内容做成可直接打开、可交互的自包含 HTML 说明页 | 2026-06-04 |
| blog-reader | 把博客/帖子网址读出正文,直接出中文 HTML 讲解页给你看 | 2026-06-04 |
| prompt-optimizer | 用 4-D 方法把粗略需求优化成适配多平台的精确提示词 | 2026-07-11 |
| quick-walkthrough | 用简明导览和具体例子快速建立项目或概念的心智模型 | 2026-07-14 |

<!--
追加示例(复制这行,去掉注释,填好):
| grill-me | 逐条拷问把方案钉死 | 2026-06-04 |
-->

## 安装

用 [`npx skills`](https://github.com/vercel-labs/skills)(vercel-labs 的开放 skill 安装工具)直接从本仓库装,无需手动拷贝目录:

```bash
# 装单个 skill(推荐,按需取用)
npx skills add Cora093/my-skills --skill blog-reader

# 一次装多个
npx skills add Cora093/my-skills --skill teach-me --skill html-explainer

# 先看看仓库里有哪些
npx skills add Cora093/my-skills --list

# 全部装上
npx skills add Cora093/my-skills --all
```

默认装到当前项目;加 `-g` 装到全局,加 `-a claude-code` 指定目标 agent,加 `-y` 跳过交互(适合 CI)。
