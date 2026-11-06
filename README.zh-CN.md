# fuyao-mac-cleanup

**在 AI agent 动手删东西之前。**

一个 macOS 腾空间 skill——dry-run 先行、硬红线、任何不可逆动作之前必须先拿到同意。它调度 [Mole](https://github.com/tw93/Mole)，不自己重写清理逻辑：一次删错的代价比一个依赖高得多。

> **语言：** [English](./README.md) | 中文（简体）
>
> 由 **扶摇（Fuyao）** 维护。属于 `fuyao-mac-*` 系列——其余的都是零安装，见 [fuyao-mac-kit](https://github.com/fuyaoai/fuyao-mac-kit)。

---

## ⚠️ 这一个需要装依赖

和其他 `fuyao-mac-*` 不一样，它需要外部引擎：

```bash
brew install mole     # https://github.com/tw93/Mole — MIT 开源
```

**skill 不会偷偷装。** 流程写在 `SKILL.md` 里：

1. **检测** —— `command -v mo`
2. **解释** —— Mole 是什么、会执行什么、怎么卸掉
3. **征求同意** —— 等一句明确的「可以」。没说就当没说
4. **安装并验证** —— `brew install mole`，再 `mo --version`
5. **用户拒绝** —— 切到只读诊断：量空间、找大户、给报告和手工指引。**拒绝装依赖不是失败状态**

机器上没有 Homebrew？skill 会请用户自己按 brew.sh 的官方指引装。它不会替用户把远端脚本管道进 shell——那不在一件清理任务该有的权限范围内。

## 它到底多提供了什么

Mole 是个好引擎，但它不给判断力。这个 skill 补的是：

| 补的东西 | 为什么重要 |
|---|---|
| **永远先 dry-run** | 每次 `clean`/`purge` 都先跑 `--dry-run`，把清单给你看，你点头才真删 |
| **硬红线** | 聊天记录、agent 的记忆目录、用户的项目文件夹——绝不自动删，只报告 |
| **补漏检查** | 微信数据、iOS 备份、安装包残留、agent 会话日志——Mole 不列举的空间大户 |
| **SIP 保护区** | 苹果 AI 模型（8–30GB）`du`、`mo clean`、`rm` 都碰不到，skill 知道真正的出路在哪 |
| **`git filter-repo` 警告** | 它会硬重置工作区——被剔除路径匹配的文件是真删，不只是改历史 |
| **活跃项目识别** | 你今天还在 build 的仓库里的 `dist`/`__pycache__`，属于「能删但最好别删」 |

## 用法

装好之后，直接说人话：

> 「盘满了，帮我清一下」
> 「到底是什么占了空间？」
> 「把旧项目的 node_modules 清掉」

agent 会先量、再报告可释放多少、等你点头才动手。

## 环境要求

- macOS
- [Mole](https://github.com/tw93/Mole) v1.55+（`brew install mole`）
- 可选：`jq`，用来解析 `mo analyze --json` 的输出

## 授权

MIT —— 见 [LICENSE](./LICENSE)。
