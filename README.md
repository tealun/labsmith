# Labsmith

让 AI Agent 从零做出**测得准、讲得对**的行业实训室，以及一个能被搜到、经得起核查的指南站。

Labsmith 是一个目录型 `SKILL.md` 技能包，面向浏览器里的**行业实训室**：用三维交互仿真真实的量具、工具、仪器或操作流程（卡尺、千分尺、扭力扳手、万用表、移液器……），再配一套把学习者带进实训室的指南文章。它来自 [KELIBRON 精密测量实训室](https://kelibron.com) 的开发过程，把其中验证过的做法整理成**分阶段的开发路线、不可违反的规则、每个阶段的通过条件和模板**。

> [English summary](#english)

> [!IMPORTANT]
> **强烈建议同时安装 [Tealun Skills](https://github.com/tealun/Tealun-Skills)。**
> Labsmith 提供实训室这一类产品的做法；规划、按契约执行、修复缺陷、审计、沉淀经验这些贯穿各阶段的工作纪律，由 Tealun Skills 的 `planner`、`tasker`、`bugfixer`、`auditor`、`evolver` 负责。不装它们，智能体无法保证在不同阶段都做到"有计划、有证据、能复现、可追溯"。Labsmith 会在每次开始时检查它们是否已安装，缺失时提醒你并按最低替代流程工作，但替代流程不等于这些技能本身。

## 它解决什么问题

做这类项目，最容易出的问题往往不是"做不出来"，而是"看起来对、其实不对"：

- 仪器模型很漂亮，但读数由渲染层随手算出来，操作不规范时照样判"合格"。
- 错误用法演示凭想象编，和行业里真实会犯的错误对不上。
- 镜头一切换就瞬移，读数视角关掉后回不到原来的位置，悬浮提示挡住刻度。
- 指南文章写得通顺，却引用了不存在的标准条款，或者把"规划中"的功能写成已有。
- 实训室更新了，文章、场景链接和演示视频没跟上。
- 没有阶段和验收标准，做到一半才发现基础模型要推倒重来。

Labsmith 把这些坑变成默认规则、阶段路线和交付检查。

## 安装

**第一步：安装 Tealun Skills（强烈建议）**

```bash
git clone https://github.com/tealun/Tealun-Skills.git
cd Tealun-Skills
bash ./sync-skills.sh            # Windows：.\sync-skills.ps1
```

同步脚本会找到本机的 Claude、Codex、Cursor、Gemini、Kimi、OpenCode 等工具的技能目录；先用 `DRY_RUN=1 bash ./sync-skills.sh` 预览更稳妥。详见该仓库的 README。

**第二步：安装 Labsmith**

```bash
git clone https://github.com/tealun/labsmith.git
```

把其中的 `labsmith/` 目录复制到 Agent 工具的技能目录：

| 工具 | 复制到 |
| --- | --- |
| Claude Code（个人） | `~/.claude/skills/labsmith/` |
| Claude Code（只在某个项目里用） | `<项目>/.claude/skills/labsmith/` |
| Codex | `~/.codex/skills/labsmith/` |
| 共享目录 | `~/.agents/skills/labsmith/` |

```bash
# macOS / Linux
mkdir -p ~/.claude/skills && cp -R labsmith/labsmith ~/.claude/skills/
```

```powershell
# Windows PowerShell
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse -Force .\labsmith\labsmith "$HOME\.claude\skills\"
```

也可以把 `labsmith/` 放进 Tealun-Skills 仓库根目录，用它的同步脚本一起安装。装完后**重新打开 CLI 或新建会话**，让工具加载技能。

## 从零开始：跟着智能体一步步做

新建一个空文件夹，在里面打开 Agent，然后按阶段推进。每个阶段都有明确的交付物和**通过条件**，没通过不要进入下一阶段。完整说明见 [`labsmith/references/build-path.md`](labsmith/references/build-path.md)。

| 阶段 | 做什么 | 主要技能 | 可以这样对智能体说 |
| --- | --- | --- | --- |
| S0 规划 | 学习者与目标、首版范围、行业概念对照、运行时契约、锁定技术栈、体验规范、项目说明、空项目与检查命令 | `planner` + `labsmith` | "用 planner 和 labsmith 为〈行业 / 仪器〉做一个面向〈学习者〉的实训室规划，先出规划文档和项目说明，不写业务代码。" |
| S1 领域核心 | 仪器与工件定义、第一个任务的接触与读数、有效性、公差判定、命令与拒绝、分享状态，全部有测试 | `tasker` | "用 tasker 和 labsmith，按运行时契约实现〈仪器〉的领域层和〈任务〉，附测试，不做界面。" |
| S2 仪器模型与读数 | 按真实尺寸建模，刻度 / 表盘 / 数显由领域数据生成，读数视角可来回切换 | `tasker`、`bugfixer` | "用 tasker 和 labsmith 建〈仪器〉的三维模型，把领域读数显示在刻度上，并做读数视角。" |
| S3 交互与镜头 | 拖动、放置与取下、锁紧，平滑过渡，触屏长按，避免手势冲突 | `tasker`、`bugfixer` | "用 tasker 和 labsmith 让仪器可以手动操作，桌面和手机都能完成一次有效读数。" |
| S4 界面与多语言 | 导航、测量面板、工件托盘、任务列表、反馈，多语言与一致性测试 | `tasker` | "用 tasker 和 labsmith 做导航、测量面板、工件托盘和任务列表，中英双语。" |
| S5 更多任务与错误用法 | 每个任务的错误用法演示、引导卡、镜头聚焦与返回 | `tasker`、`auditor` | "用 tasker 和 labsmith 增加〈任务〉和错误用法〈列表〉，含引导卡和镜头聚焦。" |
| S6 场景与短视频 | 场景深链与测试、分享链接、自动录制 5 秒实训画面 | `tasker` | "用 tasker 和 labsmith 为每个任务和演示建场景预设、测试，并录制短视频。" |
| S7 指南站与内容体系 | 关键词矩阵、生成器、首批 5 篇带来源的文章、写作 / 回顾 / 审计三份指引 | `tasker`、`auditor` | "用 tasker 和 labsmith 搭指南站，写首批 5 篇带参考资料的文章，并生成三份内容指引。" |
| S8 发布与运营 | 安全响应头、部署、域名与搜索平台提交、回顾与审计节奏、经验入账 | `auditor`、`evolver` | "用 auditor 检查发布准备，再用 labsmith 配置部署；上线后用 evolver 记录经验。" |

上线后，每增加一项能力都走同一个循环：领域与测试 → 模型、交互和界面 → 错误用法演示 → 场景和视频 → 文章更新 → 检查与发布 → 经验入账。

换到其他行业（扭力工具、电工仪表、移液器、焊接……）时，先读 [`adapting-to-a-trade.md`](labsmith/references/adapting-to-a-trade.md)：把你的行业对应到"仪器—对象—读数—有效条件—规格"这个模式，并按其中的安全规则处理有风险的操作。

## 包含什么

```text
labsmith/
├── SKILL.md                         # 开场检查、配套技能与替代流程、规则、通过条件、汇报方式
├── agents/openai.yaml               # Codex 等工具的界面元数据
├── references/
│   ├── build-path.md                # 从零到上线的 S0–S8 阶段路线与上线后的功能循环
│   ├── adapting-to-a-trade.md       # 换行业：概念对照表（卡尺 / 扭力扳手 / 万用表 / 移液器）、步骤、安全规则
│   ├── lab-engineering.md           # 领域真值层、单位、有效性、错误用法演示、交互、镜头、布局、多语言、三维界面验证
│   ├── scenes-and-media.md          # 场景深链及其测试、录制 5 秒实训短视频与封面
│   ├── guide-site.md                # 生成式指南站：首页 / 分类 / 文章、版式、内链、跳转、链接检查
│   ├── seo-geo.md                   # 人群与场景关键词矩阵、一页一意图、结构化数据、站点地图、llms.txt、上线后工作
│   ├── content-lifecycle.md         # 文章写作、定期回顾、内容审计（证据等级、来源链接规则）、与实训室版本联动
│   └── security-and-deploy.md       # 部署范围、安全响应头与 CSP、嵌入策略、参数处理、本地测试响应头
└── templates/
    ├── project-instructions.md      # 项目给智能体的说明文件（CLAUDE.md / AGENTS.md）骨架
    ├── runtime-contract.md          # 运行时契约：单位、仪器、对象、任务、有效性、公差、错误用法、命令
    ├── decision-card.md             # 技术栈变更的决策卡
    ├── authoring-guide.md           # 《专题文章创作与更新指南》骨架
    ├── review-guide.md              # 《站点回顾更新指引》骨架
    ├── audit-guide.md               # 《内容审计指引》骨架
    └── audit-log.md                 # 审计日志模板
```

**参考资料**是按需加载的详细做法：智能体按当前任务只读需要的那份。**模板**是给你的项目生成文档用的起点。

### 七条不可违反的规则

1. 测量结果只有一个来源：与框架无关的领域层；界面和渲染只发命令、画状态。
2. 任何错误设置都不能判为合格；错误用法演示永远不参与公差判定。
3. 真实单位、真实尺寸；仪器与对象数据在专家审核前保持"草稿"。
4. 错误用法按行业真实做法编排，原因、影响、纠正都有依据；有安全风险的行业绝不把不安全操作呈现为可接受。
5. 文章中的标准、数值、经验规则、人物和产品陈述必须有来源，否则收窄说法。
6. 遵守项目锁定的技术栈；只为内容制作服务的工具不进依赖、不进部署包。
7. 所有语言版本事实一致，各自地道表达。

## 与 Tealun Skills 的分工

| 角色 | 技能 | 用在哪些阶段 |
| --- | --- | --- |
| 规划、架构、文档体系、就绪门槛 | `planner` | S0；每次新增仪器或重大功能前 |
| 把每次改动当作可检查的契约执行，并给出证据 | `tasker` | S1–S7 与每个功能循环 |
| 从症状复现并按原因修复缺陷 | `bugfixer` | 检查失败或"还是不对"时 |
| 有证据的代码、安全与发布审计 | `auditor` | S5、S7、S8 结束时与每次发布前 |
| 把验证过的经验固化为规则、检查和台账 | `evolver` | S8 之后，以及反复纠正之后 |

## 一个完整的例子

[KELIBRON](https://kelibron.com) 是用这套做法做出来的：

- 实训室：数显、带表、游标三种卡尺，外径、内孔、深度三类任务，每类任务都有对应的错误用法演示与引导纠正；
- 场景深链：`/zh/?scene=error-tilt` 直接打开一个错误用法演示，每个预设都有测试保证；
- 指南站：[kelibron.com/zh/guide/](https://kelibron.com/zh/guide/)，首页 / 分类 / 文章三级结构，每篇配实训短视频、练习场景和参考资料。

模板中提到的"示例"是 KELIBRON 项目里按这些骨架写成的指引；模板本身已列出每一节要写什么，没有示例也能直接填写。

## 设计原则

- **证据优先**：读数要在浏览器里实际读到，截图或录制帧才算验证；文章结论要有来源或可重复的计算。
- **阶段推进**：先小后大，每个阶段有通过条件，没通过不进入下一阶段。
- **领域优先**：先在领域层建模并写测试，再接界面。
- **内容跟着实训室走**：每项新能力都对应场景、视频和文章，并有回顾与审计流程兜底。

## 版本

见 [CHANGELOG.md](CHANGELOG.md)。

## 许可证

MIT License，详见 [LICENSE](LICENSE)。

---

## English

**Labsmith** is a directory-style `SKILL.md` skill that takes an AI agent from an empty folder to a published **industry training lab** — an interactive 3D simulation of a real instrument, tool or procedure — and the guide site that brings learners to it. It was distilled from building the [KELIBRON](https://kelibron.com) precision-measurement lab.

> **Strongly recommended:** install the [Tealun Skills](https://github.com/tealun/Tealun-Skills) (`planner`, `tasker`, `bugfixer`, `auditor`, `evolver`) first. Labsmith supplies the domain playbook; those skills supply the planning, evidence-backed execution, defect repair, auditing and lesson capture that each stage depends on. Labsmith checks for them at the start of every session, warns when one is missing and applies a minimal fallback — which is not a substitute.

It covers:

- **Build path:** stages S0–S8 (planning, domain core, instrument and readout, interaction and camera, interface and languages, tasks and wrong-use demos, scenes and clips, guide site and content system, release and operations), each with what to tell the agent, deliverables and an exit gate; then a feature loop for every new capability.
- **Adapting to a trade:** mapping any trade onto instrument → subject → reading → validity → specification, with worked maps for calipers, torque wrenches, multimeters and pipettes, and safety rules.
- **Lab engineering, scenes and media, guide site, SEO/GEO, content lifecycle, security and deployment** — the practices behind each stage.
- **Templates:** project instructions, runtime contract, decision card, and authoring / review / audit guides with an audit log.

**Install:** Tealun Skills first (`git clone https://github.com/tealun/Tealun-Skills.git && bash Tealun-Skills/sync-skills.sh`), then copy this repository's `labsmith/` folder into your agent's skills directory (Claude Code: `~/.claude/skills/labsmith/`) and start a new session. **Use:** `/labsmith start a training lab for <trade> from zero` or "use labsmith to …".

License: MIT.
