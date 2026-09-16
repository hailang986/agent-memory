# AgentMemory

AgentMemory 是给 AI coding agents 使用的轻量共享记忆层。

它是一组不同 Agent 都可以读写的 Markdown 文件。文件保存当前事实，Git 保存历史。

**Markdown = 当前事实**
**Git = 历史**

本仓库是从私人工作流中提炼出的公开、通用实现，不包含原私人知识库或其 Git 历史。

## 要解决的问题

Claude Code、Codex 等编码 Agent 各自保存短暂上下文。那套上下文不是共享、可检查、可版本控制的项目记录。

如果没有外部事实层：

- 两个 Agent 可能记住不同的“当前状态”
- Agent 私有记忆可能悄悄偏离磁盘上的文件
- 用户很难审计、回滚或交接长期项目知识

AgentMemory 把持久事实放在普通 Markdown 里，让人和 Agent 共用同一套真相。

## 架构

设计刻意保持很小。

- Markdown 文件是唯一长期事实源。
- Git 保存这些文件的历史版本。
- Agent 自带 Memory 不是第二套知识库。
- 工具适配层只用于避免 Claude Code / Codex 另建一套私有长期记忆。

当前版本不依赖数据库、向量索引或隐藏运行时状态。

## 目录结构

```text
.
├── README.md
├── README.zh-CN.md
├── LICENSE
├── AGENTS.md
├── INDEX.md
├── USER.md
├── CLAUDE.md
├── .gitignore
├── .claude/
│   └── settings.json
├── .codex/
│   └── config.toml
├── templates/
│   ├── USER.md
│   └── project/
│       ├── PROJECT.md
│       ├── CURRENT.md
│       ├── DECISIONS.md
│       └── PITFALLS.md
├── examples/
│   └── weather-cli/
│       ├── PROJECT.md
│       ├── CURRENT.md
│       ├── DECISIONS.md
│       └── PITFALLS.md
└── docs/
    ├── architecture.md
    ├── privacy.md
    ├── codex.md
    └── claude-code.md
```

在实际使用的知识库中，真实项目放在 `projects/<project>/`，文件职责与示例相同。

## 快速开始

1. 复制本目录，或把 `AGENTS.md`、`INDEX.md`、`USER.md` 和项目模板拷到新目录。
2. 只在 `USER.md` 填写非敏感的长期偏好。
3. 用 `templates/project/` 创建 `projects/<project>/`。
4. 让 Claude Code 或 Codex 指向该目录。
5. 把 Markdown 当作当前事实。准备好版本管理时，再用 Git 保存历史。

不要在这些文件里存放凭据、身份材料或私人证据。

## 文件职责

| 文件 | 作用 |
| --- | --- |
| `AGENTS.md` | Agent 行为与知识库治理。 |
| `INDEX.md` | 仅作导航。 |
| `USER.md` | 已确认、稳定、非敏感的用户偏好。 |
| `PROJECT.md` | 项目长期定义：目标、范围、原则、约束、架构。 |
| `CURRENT.md` | 唯一当前状态：阶段、已完成、进行中、下一步、阻塞项。 |
| `DECISIONS.md` | 对未来持续有影响的重要决策。 |
| `PITFALLS.md` | 实际发生、已验证、且可能复发的问题。 |

`CURRENT.md` 随项目推进原地更新。旧状态属于 Git，不堆在状态文件里。

## Codex 接入

Codex 应以 `AGENTS.md` 为主要治理入口。

`.codex/config.toml` 只是项目级薄适配。本候选版本里，它只关闭 Codex 私有 memories，避免与 Markdown 知识库形成双重真相。

它不会自动扫描文件、拦截 secrets，也不会自动执行隐私策略。隐私仍然依赖 Agent 行为治理和人工审查。详见 `docs/codex.md`。

## Claude Code 接入

Claude Code 应以 `CLAUDE.md` 为项目指令入口。

`CLAUDE.md` 只有 `@AGENTS.md`，用来导入公共治理规则。`.claude/settings.json` 是项目级薄适配，用于关闭 Claude Code auto-memory。

不要把 `AGENTS.md` 正文复制进 `CLAUDE.md`。详见 `docs/claude-code.md`。

## 隐私边界

这些文件会被人、Agent，以及之后的 Git 读取。

不要写入：

- 密码、token、cookie、session、私钥、OTP 或恢复码
- 政府身份证件号或完整金融账户号
- 原始财务导出、身份材料或私人截图
- 真实本机路径、hostname、内网 IP 或私有 endpoint

如果 Secret 曾经进入 Git 历史，删除当前文件并不够。应先 revoke / rotate，再单独处理历史。

详见 `docs/privacy.md`。

## 示例项目

`examples/weather-cli/` 是一个完全虚构的天气 CLI，只用来展示四份项目文件如何配合。

它不是真实产品，也不包含私人项目、真实设备或财务数据。

## 限制

- 这是文件约定，不是托管服务。
- 它不会自动检测 secrets。
- 它不会替你同步 Agent 私有记忆；适配层只尽量避免那套记忆变成第二事实源。
- Agent 仍然需要权限和纪律：写入前先读文件。
- Git 是历史层。使用正常的 Git 提交保存历史、审查变更，并在需要时回滚。

这里不提供用户数、下载量、benchmark 或生产采用数据。
