---
name: lark-cli-router
description: >-
  通过 lark-cli 内嵌且与当前版本匹配的领域 skill 路由所有飞书 / Feishu / Lark 请求。
  当用户要处理飞书资源、URL、token 或操作时使用，覆盖云文档、云盘、电子表格、多维表格、
  知识库、消息、邮箱、日历、会议、任务、审批、联系人、OKR、考勤、应用和工作流；
  也用于 lark-cli 安装配置、身份认证、权限和原生 OpenAPI 调用，包括 feishu.cn、
  larksuite.com 和 doubao.com 下的飞书资源。
---

# 飞书 CLI 路由器

将当前安装的 `lark-cli` 所内嵌的 skills 作为唯一业务知识来源。只加载本次请求需要的领域，
再严格执行该领域 skill 的规则。

## 路由并执行

### 1. 识别领域

执行：

```powershell
lark-cli skills list
```

根据返回的名称和描述选择范围最小的匹配领域。先选一个主 skill；仅当主 skill 将子操作
路由到另一领域，或请求确实跨领域时，再加入其他 skill。若 `lark-cli` 不存在或
`skills list` 失败，直接说明缺失的前置条件，不要根据旧名称猜测。

完成条件：所选 skill 的描述同时覆盖用户要操作的资源和操作意图。

### 2. 加载权威规则

执行任何领域操作前，完整读取目标 skill：

```powershell
lark-cli skills read <skill-name>
```

始终读取 CLI 内嵌版本，不用同名的全局 skill 替代；内嵌版本与当前 CLI 的命令和 schema
保持同步。

完成条件：已读完目标 `SKILL.md`，并明确其身份、认证、前置条件和路由规则。

### 3. 解析所有必读引用

执行对应操作前，读取领域 skill 标记为必读的每个文件。按下表转换其中的链接：

| 内嵌 skill 中的链接 | 读取命令 |
| --- | --- |
| `references/file.md` | `lark-cli skills read <current-skill> references/file.md` |
| `../lark-shared/SKILL.md` | `lark-cli skills read lark-shared` |
| `../lark-foo/SKILL.md` | `lark-cli skills read lark-foo` |
| `../lark-foo/references/file.md` | `lark-cli skills read lark-foo references/file.md` |

遇到新选中的跨领域 skill 时，从第 2 步继续。条件引用只读取当前操作分支需要的文件；
该分支上标为 `required`、`MUST`、"必读"或同等强度的引用必须全部读取。

`lark-cli skills` 只内嵌知识文件，不内嵌 `scripts/`、`assets/` 等机器资源。
当领域 skill 要求使用此类资源时：

1. 在该领域 skill 的已安装副本中查找完全相同的相对路径。
2. 文件存在且适用于当前操作时，使用该文件。
3. 文件缺失时停止该操作分支，并明确报告缺少的机器资源；不要凭记忆重建可执行代码或模板。

完成条件：当前分支要求的所有知识文件均已读完，所有机器资源均已定位。

### 4. 执行领域规则与安全门禁

执行领域 skill 指定的命令。优先使用其 `+shortcut`；没有合适 shortcut 时使用 typed
service command；仅在前两者都不适用且领域规则明确允许时使用原生 OpenAPI。命令参数或
风险等级不明确时，先读取 `--help` 或 `schema`。

保持领域 skill 选定的身份。认证、scope、本地文件处理和 notice 均遵循 `lark-shared`。

将退出码 `10` 且 `error.type == "confirmation_required"` 视为强制确认门禁：

1. 向用户展示风险操作及关键参数。
2. 等待用户明确同意或拒绝。
3. 用户同意后，在原参数列表末尾追加 `--yes`，然后重试一次。
4. 用户拒绝后结束操作，保持原参数不变。

用户需要预览高风险请求时，先使用 `--dry-run`。始终以结构化参数直接调用
`lark-cli`，不要将用户输入插值到 shell 命令字符串中。

完成条件：请求的读写操作已成功、已得到用户明确决定，或已返回可行动的错误。

### 5. 验证结果并继续路由

检查命令的结构化结果，不要只依赖进程退出状态。将需要认证、授权、升级或用户处理的
`_notice` 告知用户。写操作需验证返回的资源标识或状态；领域 skill 要求回读时执行回读。

若结果中出现另一种飞书资源，从第 1 步为该子操作重新路由，并在继续前加载对应的内嵌
skill。

完成条件：范围内每个领域操作都有已验证的结果，或都有明确说明的阻塞原因。
