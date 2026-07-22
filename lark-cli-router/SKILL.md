---
name: lark-cli-router
description: >-
  通过当前 lark-cli 内嵌的领域 skill 处理所有飞书 / Feishu / Lark 请求。用户要操作飞书
  资源、URL、token，或处理 lark-cli 安装、认证、权限、OpenAPI 时使用；也适用于
  feishu.cn、larksuite.com 和 doubao.com 下的飞书资源。
---

# 飞书 CLI 路由器

本 skill 只负责路由；具体操作以当前 `lark-cli` 内嵌的领域 skill 为准。

## 路由并执行

1. 查看当前 CLI 提供的领域：

```powershell
lark-cli skills list
```

2. 根据列表中的描述选择范围最小的匹配 skill，并在操作前完整读取：

```powershell
lark-cli skills read <skill-name>
```

3. 按领域 skill 的要求读取当前操作分支的必读引用。使用其末尾说明的
   `lark-cli skills read` 方式读取，不用同名全局 skill 或旧知识替代。

   `scripts/`、`assets/` 等机器资源不在 CLI 内嵌内容中；从该领域 skill 的已安装副本按
   原相对路径使用，找不到就停止相关操作，不要自行重建。

4. 严格按已加载规则执行并验证结果。领域 skill 要求切换到其他 skill 时，先完整读取新
   skill；请求跨领域时只为实际子操作按需增加 skill。

若 `lark-cli`、`skills list` 或必读资源不可用，说明具体前置条件并停止受影响的操作，
不要猜测命令或绕过确认、认证和权限规则。
