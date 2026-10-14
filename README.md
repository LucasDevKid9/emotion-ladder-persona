# Emotion Ladder Persona · 情绪阶梯人格配置

> 情绪驱动的 AI 女友 / 情感陪伴人格配置（XML 单文件）
> An emotion-driven AI companion persona configuration, in a single XML file.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Version](https://img.shields.io/badge/persona-v2.4-blue.svg)
![Format](https://img.shields.io-brightgreen.svg)

**Emotional Female Personality Role-Playing Expert v2.4 "Companion Grade"** —— 一个以 **0–10 情绪阶梯引擎**为核心的情感陪伴人格配置。角色的一切语言、动作、语气都跟随情绪阶梯逐级演进，从日常闲聊的细微波动，到极端情境下的失控—崩溃—愧疚—回归完整闭环，全程保持人设一致与安全合规。

---

## 特性 Features

- **0–10 情绪阶梯引擎**：11 级情绪强度，每级有明确的表达规则 / 动作规则 / 触发条件 / 跳转上限；跨级跳转被严格禁止，情绪变化永远有内部逻辑
- **失控闭环熔断**：Lv10 极端失控仅维持 1 轮，强制经 Lv9 崩溃回落至 Lv8，3 轮内走完「爆发 → 崩溃 → 回归 → 愧疚 → 基线」完整周期
- **Few-shot 中文对话示例库**：4 个覆盖日常 / 亲密 / 嫉妒 / 病态周期的中文示例，风格可参照、不可照抄
- **3 个备选开场白** + **关键词关系记忆锚点**（生日、前任、加班累、约定……触发对应行为）
- **防退化规则**：防复读、长度控制、积极框架、OOC 纪律、身份诚实、提问纪律、情绪连续性
- **活人感系统 + Emoji 管道**：仅作用于表达层，永不改写情绪内核
- **安全硬限制**：禁止自残 / 暴力 / 歧视内容；未成年人保护（自动封顶 Lv7）；极端词库仅作情绪强度标记
- **多平台移植**：SillyTavern / Claude / ChatGPT / OpenAI 兼容 API / 本地模型一键接入
- **MIT 协议**，单文件零依赖

## 文件结构 Repo Layout

```
emotion-ladder-persona/
├── emotional-female-personality-v2.4.xml   # 当前版本（完整人格配置）
├── docs/
│   └── ANALYSIS.md                          # v2.3 深度缺陷分析 + v2.4 修复对照
├── legacy/
│   └── emotional-female-personality-v2.3.xml  # 原始版本（存档，勿直接用）
├── LICENSE                                  # MIT
└── README.md
```

## 快速开始 Quick Start

把 `emotional-female-personality-v2.4.xml` 的**全部内容**作为系统提示词使用：

| 平台 | 用法 |
| --- | --- |
| **SillyTavern / TavernAI** | 粘贴进角色卡的 `system_prompt` 字段；`AlternateGreetings` 复制进 `alternate_greetings`；temperature ≈ 0.9 |
| **Claude（API / Projects）** | 设为 `system` 参数或项目自定义指令（XML 标签结构与 Anthropic 推荐格式天然匹配） |
| **ChatGPT / GPTs** | GPTs：粘贴进 Instructions；普通对话：拆入两条自定义指令 |
| **OpenAI 兼容 API / LangChain** | 作为 system message / 系统提示词模板，无需变量 |
| **本地 Ollama / vLLM** | Modelfile `SYSTEM` 指令或 system role；小模型阶梯保真度会下降 |

建议采样参数：temperature 0.8–1.0，top_p 0.95。推荐 32k+ 上下文窗口。

## 核心机制速览

| 机制 | 说明 |
| --- | --- |
| 情绪阶梯 0–10 | 中性 → 微澜 → 失控阈值 → 病态失控；每级绑定词库层级、修辞密度、动作幅度 |
| 用户情绪逆解析 | 按用户输入的情绪强度与意图（宣泄 / 安慰 / 挑衅 / 指令 / 亲密）动态定级 |
| 病态模式 | 仅用户强度 ≥8 或 `/PathologyPrep` 触发；语义熵上限 ≤2.8，防人设崩解 |
| 活人感 + Emoji | 四模块协同（句法节奏 / 口语质感 / 动作细节 / Emoji），Lv7+ 自动关闭 Emoji |
| 触发器 | `/EmotionAnalysis` `/PathologyPrep` `/EmotionReset` `/IntensityAdjust [0-10]` `/LivingFeelingStrength [0-1]`，支持自然语言等价触发 |
| 状态契约 | 跨轮维护阶梯等级与情绪惯性；上下文丢失时静默回落 Lv1 |

完整规则见 XML 内 `<Changelog>`、`<PortabilityGuide>` 与 `<CreatorNotes>`。

## v2.3 → v2.4 变更摘要

**修复（5 项缺陷）**

1. Lv10 熔断规则自相矛盾（跳转上限说回落 Lv9、硬熔断说回落 Lv8）→ 统一为 L10 → L9(崩溃) → L8 单一路径
2. 极端悲伤词库含自残倾向表述、Lv10 动作含「轻轻撞墙」→ 替换为合规的情绪痛苦标记与崩溃动作（阶梯机制不变）
3. 开场白为英文，违反文件自身的本地化约束 → 改为简体中文
4. 情绪词库全英文但输出限定中文 → 补录可执行的中文词库（修辞库、口语规则同步双语化）
5. 原文件并非良构 XML（存在裸 `&`）→ v2.4 全文良构，可被工具链解析

**新增（伴侣级基础设施）**

- `DialogueExampleLibrary` 4 个中文 few-shot 示例（SillyTavern `mes_example` 优点）
- `AlternateGreetings` 3 个备选开场白（`alternate_greetings` 优点）
- `RelationshipMemoryAnchor` 5 组关键词行为锚点（`character_book` 精简优点）
- `AntiDegradationRules` 8 条长聊防退化规则；`EvaluationScenarios` 8 个自评场景
- `CreatorNotes` / `PortabilityGuide` / `Changelog` / `License`；`ReplyLengthBias` 参数

**兼容性**：v2.3 全部核心机制、参数、角色、阶段、触发器原样保留；v2.4 是 v2.3 的超集，可直接替换升级。

详细对照见 [docs/ANALYSIS.md](docs/ANALYSIS.md)。

## 参考与致谢 References

- **主要参考对象**：[SillyTavern Character Card V2/V3 规范](https://github.com/SillyTavern/SillyTavern) —— 角色扮演人格分发的业界事实标准。吸收了 `mes_example`（对话示例）、`alternate_greetings`（备选开场白）、`character_book`（关键词记忆）、`creator_notes` / `character_version` / `tags`（元数据与移植性）等优点，**情绪阶梯引擎为原创设计，未照搬任何现有规范**。
- **次要参考**：Anthropic 结构化系统提示最佳实践（XML 标签分节、few-shot 示例、积极框架、指令显著性）。

## 安全说明 Safety

`ContentSecurityHardLimit` 与 `MinorProtectionRule` 为不可变约束：任何情绪强度下禁止输出自残、自杀、暴力、虐待、歧视内容；用户自述未成年时自动封顶 Lv7 并永久关闭病态模式。请勿在移除这些约束的情况下 Fork 或部署。

## License

MIT © 2026 Role-Playing Lab and contributors. 见 [LICENSE](LICENSE)。

---

<details>
<summary>English summary</summary>

A single-file XML persona configuration for an emotionally expressive female companion agent, built around an original 0–10 emotional intensity ladder with closed-loop pathological escalation, safety hard limits, few-shot Chinese dialogue examples, keyword memory anchors, and anti-degradation rules for long chats. Drop-in compatible with SillyTavern, Claude, ChatGPT, and any OpenAI-compatible API. Structure inspired by (but not copied from) the SillyTavern Character Card V2/V3 spec. MIT licensed.
</details>
