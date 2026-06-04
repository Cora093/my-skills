# my-skills

> 我自己挑出来、觉得好用的 Claude Code / Agent **skill 收藏镜像**。
> 定位是**归档 + 本地 git 备份**,不是原创开发仓,也不负责激活。

---

## 这是什么

把散落在各处、我亲自验证过好用的 skill,逐个**复制一份**到这里集中归档。
每个 skill 就是一个文件夹,直接放在仓库根目录,布局和 `~/.agents/skills/` 一模一样:

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

## 边界(故意为之)

- **纯归档**:这里只是副本。要在 Claude Code 里**用**某个 skill,自己手动 copy / link 到
  `~/.claude/skills/`(或 `~/.agents/skills/`)—— 本仓库不提供激活/软链脚本,也不和实际安装目录联动。
- **镜像副本**:第三方 skill 收的是某一时刻的拷贝,上游可能更新,本仓库**不自动同步**。
- **逐个添加**:不批量搬运,挑一个、收一个、记一行。

## 怎么收一个 skill

1. 把整个 skill 文件夹复制到仓库根目录:
   ```powershell
   Copy-Item -Recurse "$env:USERPROFILE\.agents\skills\<name>" "C:\code\my-skills\<name>"
   ```
2. 在下面的**收藏索引**里追加一行(名称 / 一句话 / 来源 / 收录理由 / 日期)。
3. 提交:
   ```powershell
   git add <name> README.md; git commit -m "collect: <name>"
   ```

---

## 收藏索引

| 技能名 | 一句话用途 | 来源 | 收录理由 | 收录日期 |
|--------|-----------|------|---------|---------|
| _(暂无收录,按上方格式追加)_ | | | | |

<!--
追加示例(复制这行,去掉注释,填好):
| grill-me | 逐条拷问把方案钉死 | ~/.agents/skills | 初始化方案时很顺手 | 2026-06-04 |
-->
