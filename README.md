**中文** | [English](./README_EN.md)

# Gemini CLI Skill for Claude Code

## 简介

本插件将 Google Gemini CLI 变为 Claude Code 的一流 AI 同事。Gemini 不再是被动调用的工具，而是 Claude 主动借力的智能搭档——弥补自身短板：实时网络搜索、200 万 token 超长上下文、音频处理、并发子智能体，以及推理交叉验证。

## 前置要求

- **平台：** 需要类 Unix shell（macOS、Linux，或 Windows 下的 Git Bash / WSL）。原生 PowerShell / CMD 不受支持。
- `gemini` CLI 已安装并在 `PATH` 中可用（运行 `gemini --version` 验证）。
- Gemini 凭证已配置：通过 `GEMINI_API_KEY` 环境变量设置 API Key，或通过 `gemini auth login` 完成 OAuth 登录。

## 安装

### 方式一：插件市场安装（推荐）

```
/plugin marketplace add qichiyuhub/skill-gemini-cli
/plugin install skill-gemini-cli@skill-gemini-cli
```

### 方式三：独立 Skill 安装

```bash
git clone --depth 1 https://github.com/qichiyuhub/skill-gemini-cli.git /tmp/skills-temp
cp -r /tmp/skills-temp/plugins/skill-gemini-cli/skills/gemini-cli ~/.claude/skills/gemini-cli
rm -rf /tmp/skills-temp
```

然后在项目 `CLAUDE.md` 中引用 skill 路径，或通过 Claude Code skill 配置启用。

## Gemini 能力对照表

| Claude 的局限 | Gemini 的优势 |
|---|---|
| 知识截止日期 | `google_web_search` — 原生 Google 搜索，获取实时信息 |
| 上下文窗口限制 | 200 万 token 上下文——一次分析整个代码库 |
| 单线程处理 | `generalist` / `codebase_investigator` 并发子智能体 |
| 无音频处理能力 | 多模态：MP3、WAV、FLAC 音频分析 |
| 图像分析受限 | 多模态：PNG、JPG、GIF、WEBP、SVG、BMP 第二视角 |
| 推理存在不确定性 | 交叉验证——提供第二个 AI 意见 |
| 无原生搜索引擎 | 事实核查、最新文档查询 |
| 陷入逻辑死循环 | 全新视角复盘——2 次以上失败后主动介入 |

## 工作原理

当 Claude 识别到符合 Gemini 优势的任务时，会**主动**调用 Gemini，无需用户说"使用 Gemini"——Claude 自主判断并执行。

### 典型工作流示例

**实时搜索：**
```
用户：React 19.1 有哪些破坏性变更？
Claude：（识别到知识截止日期后的问题，调用 Gemini 的 google_web_search）
```

**大型代码库分析：**
```
用户：梳理这个 monorepo 的所有依赖链。
Claude：（委托 Gemini 的 codebase_investigator，借助 200 万 token 上下文完成分析）
```

**交叉验证：**
```
用户：这段并发逻辑正确吗？
Claude：（先自行分析，再让 Gemini 独立验证）
```

**音频分析：**
```
用户：转录并总结这段会议录音。
Claude：（将音频文件委托给 Gemini 的多模态处理）
```

## 委托模式

| 模式 | 适用场景 |
|---|---|
| `auto_edit`（默认） | 分析、搜索、代码审查、读取 + 编辑任务 |
| `yolo` / `-y` | 用户授权完全自主，或任务需要修改文件 |
| `plan` | 只读规划与架构方案设计 |

## Skill 参考文档

完整执行规则、错误处理与协作协议：[`plugins/skill-gemini-cli/skills/gemini-cli/SKILL.md`](plugins/skill-gemini-cli/skills/gemini-cli/SKILL.md)

## 常见问题排查

| 问题 | 解决方案 |
|---|---|
| `gemini: command not found` | 安装 Gemini CLI 并确保其在 `PATH` 中。 |
| 首次调用认证错误 | 在 Claude Code 中运行 `! gemini` 完成交互式认证，或设置 `GEMINI_API_KEY` 环境变量。 |
| 频率限制（429）错误 | Skill 会自动降级模型（`pro` -> `flash` -> `flash-lite`）。若持续出现，请检查 API 配额。 |
| 命令挂起 | 确保 `-p` 和 `--approval-mode` 参数存在，终止进程后重试。 |
| 首次调用缓慢 | 正常现象——Gemini CLI 首次运行时会初始化扩展，后续调用会很快。 |

## 许可证

MIT — 详见 [LICENSE](LICENSE).
