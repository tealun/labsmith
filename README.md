# Labsmith

让 AI Agent 做出**测得准、讲得对**的行业实训室，以及一个能被搜到、经得起核查的指南站。

Labsmith 是一个目录型 `SKILL.md` 技能包，面向浏览器里的**行业实训室**：用三维交互仿真真实的量具、工具、仪器或操作流程（卡尺、千分尺、量规、扭力工具、实验设备……），再配一套把学习者带进实训室的指南文章。它来自 [KELIBRON 精密测量实训室](https://kelibron.com) 的开发过程，把其中验证过的做法整理成可复用的规则、流程和检查项。

> [English summary](#english)

## 它解决什么问题

做这类项目，最容易出的问题往往不是"做不出来"，而是"看起来对、其实不对"：

- 卡尺模型很漂亮，但读数由渲染层随手算出来，量爪偏斜时照样判"合格"。
- 错误用法演示凭想象编，和行业里真实会犯的错误对不上。
- 镜头一切换就瞬移，读数视角关掉后回不到原来的位置，悬浮提示挡住刻度。
- 指南文章写得通顺，却引用了不存在的标准条款，或者把"规划中"的功能写成已有。
- 实训室更新了，文章、场景链接和演示视频没跟上。
- SEO 做成关键词堆砌，读者读不下去；或者完全没有结构化数据和多语言标注。

Labsmith 把这些坑变成默认规则和交付检查。

## 包含什么

```text
labsmith/
├── SKILL.md                      # 规则、工作流、交付检查、与其他技能的分工
├── agents/openai.yaml            # Codex 等工具的界面元数据
├── references/
│   ├── lab-engineering.md        # 领域真值层、单位、有效性、错误用法演示、交互、镜头、布局、多语言、三维界面验证
│   ├── scenes-and-media.md       # 场景深链及其测试、录制 5 秒实训短视频与封面
│   ├── guide-site.md             # 生成式指南站：首页 / 分类 / 文章、版式、内链、跳转、链接检查
│   ├── seo-geo.md                # 人群与场景关键词矩阵、一页一意图、结构化数据、站点地图、llms.txt、上线后工作
│   ├── content-lifecycle.md      # 文章写作、定期回顾、内容审计（证据等级、来源链接规则）、与实训室版本联动
│   └── security-and-deploy.md    # 部署范围、安全响应头与 CSP、嵌入策略、参数处理、本地测试响应头
└── templates/
    ├── authoring-guide.md        # 项目的《专题文章创作与更新指南》骨架
    ├── review-guide.md           # 项目的《站点回顾更新指引》骨架
    ├── audit-guide.md            # 项目的《内容审计指引》骨架
    └── audit-log.md              # 审计日志模板
```

### 七条不可违反的规则

1. 测量结果只有一个来源：与框架无关的领域层；界面和渲染只发命令、画状态。
2. 任何错误设置都不能判为合格；错误用法演示永远不参与公差判定。
3. 真实单位、真实尺寸；量具与工件数据在专家审核前保持"草稿"。
4. 错误用法按行业真实做法编排：原因、对读数的影响、纠正方法，都要有依据。
5. 文章中的标准、数值、经验规则、人物和产品陈述必须有来源，否则收窄说法。
6. 遵守项目锁定的技术栈；只为内容制作服务的工具（浏览器录制、ffmpeg）不进依赖、不进部署包。
7. 所有语言版本事实一致，各自地道表达，不逐句直译。

## 与 Tealun Skills 的分工

Labsmith 只负责这一类产品的领域做法，通用的工作方式交给 [Tealun Skills](https://github.com/tealun/Tealun-Skills)，两者配合使用：

| 需要 | 使用 |
| --- | --- |
| 新实训室的前期规划、技术选型、文档体系 | `planner`，再用 Labsmith 补实训室特有的决策 |
| 把一次改动当作可检查的契约来执行 | `tasker`，叠加 Labsmith 的交付检查 |
| 已出现的报错、回归、"还是不对" | `bugfixer` |
| 代码、安全、部署审计 | `auditor`（Labsmith 的内容审计只针对文章） |
| 把验证过的经验固化为规则或防线 | `evolver` |

没有安装 Tealun Skills 也能单独使用 Labsmith，只是这些环节需要你按自己的方式处理。

## 安装

Labsmith 是一个技能目录，把 `labsmith/` 复制到你的 Agent 工具的技能目录即可，然后重新打开 CLI 或新建会话。

```bash
git clone https://github.com/tealun/labsmith.git
```

| 工具 | 复制到 |
| --- | --- |
| Claude Code（个人） | `~/.claude/skills/labsmith/` |
| Claude Code（只在某个项目里用） | `<项目>/.claude/skills/labsmith/` |
| Codex | `~/.codex/skills/labsmith/` |
| 共享目录 | `~/.agents/skills/labsmith/` |

macOS / Linux 示例：

```bash
mkdir -p ~/.claude/skills
cp -R labsmith/labsmith ~/.claude/skills/
```

Windows PowerShell 示例：

```powershell
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse -Force .\labsmith\labsmith "$HOME\.claude\skills\"
```

如果你在用 Tealun Skills，也可以把 `labsmith/` 目录放进 Tealun-Skills 仓库根目录，用它的同步脚本一起安装。

其他工具的技能目录位置以各自文档为准；只要支持目录型 `SKILL.md` 技能包即可。

## 怎么用

在项目目录里打开 Agent，直接说需求。支持斜杠命令的工具可以这样指定：

```text
/labsmith 给卡尺实训室加一个"测台阶高度"任务，含错误用法演示和场景链接，并在浏览器里验证读数。
/labsmith 规划一个扭力扳手实训室：真值层怎么建模、要演示哪些错误用法、指南站分哪几类。
/labsmith 实训室新增了千分尺，按联动表更新专题文章、场景和短视频。
/labsmith 按内容审计指引审计"公差与判定"分类下的全部文章，补充来源链接并修复问题。
/labsmith 为我们的实训室写一套写作、回顾、审计三份内容指引。
```

不支持斜杠命令的工具，说"用 labsmith ……"即可。技能的 `description` 中包含"实训室、虚拟仿真实训、量具仿真、专题文章、内容审计"等触发词，多数工具会自动选中。

## 一个完整的例子

[KELIBRON](https://kelibron.com) 是用这套做法做出来的：

- 实训室：数显、带表、游标三种卡尺，外径、内孔、深度三类任务，每类任务都有对应的错误用法演示与引导纠正；
- 场景深链：`/zh/?scene=error-tilt` 直接打开一个错误用法演示，每个预设都有测试保证；
- 指南站：[kelibron.com/zh/guide/](https://kelibron.com/zh/guide/)，首页 / 分类 / 文章三级结构，每篇配实训短视频、练习场景和参考资料。

模板中提到的"示例"是 KELIBRON 项目里按这些骨架写成的三份指引（写作 `02_02`、回顾 `02_03`、审计 `02_04`）；模板本身已列出每一节要写什么，没有示例也能直接填写。

## 设计原则

- **证据优先**：读数要在浏览器里实际读到，截图或录制帧才算验证；文章结论要有来源或可重复的计算。
- **领域优先**：先在领域层建模并写测试，再接界面。
- **可回退的交互**：所有镜头和视图变化都平滑、可撤回，不做学习者没要求的跳转。
- **内容跟着实训室走**：每项新能力都对应场景、视频和文章，并有回顾与审计流程兜底。

## 版本

见 [CHANGELOG.md](CHANGELOG.md)。

## 许可证

MIT License，详见 [LICENSE](LICENSE)。

---

## English

**Labsmith** is a directory-style `SKILL.md` skill for building browser-based **industry training labs** — interactive 3D simulations of real instruments, tools or procedures — together with a guide site that brings learners to them and stays true as the lab grows. It was distilled from building the [KELIBRON](https://kelibron.com) precision-measurement lab.

It covers:

- **Lab engineering:** a framework-free domain layer as the only source of measurement truth; real units; explicit validity, so an invalid setup never passes; wrong-use demonstrations drawn from trade practice; continuous, reversible camera and interaction; i18n parity; verifying WebGL UI with Playwright.
- **Scenes and media:** deep links that open a prepared exercise, with a test for every preset; scripted 5-second lab clips (H.264 + WebP poster + JPEG preview).
- **Guide site:** a single-source generator (home → categories → articles), one stylesheet with dark/light themes, glossary internal links, tag-based related guides, redirects and a link check that fails the build.
- **SEO and GEO:** audience- and scenario-based keyword plans, one page per intent, answer-first writing, structured data (`TechArticle` with `citation` and `video`, `FAQPage`, `BreadcrumbList`, `CollectionPage`), media sitemaps, `llms.txt`.
- **Content lifecycle:** authoring, periodic review and evidence-graded content audit with publisher-only source links; keeping articles in step with lab releases.
- **Security and deployment:** deploy only the build output, CSP and other headers, framing policy, defensive parameter handling.

It deliberately does not repeat general planning, execution, debugging, code-audit or lesson-capture workflows; it hands those to the [Tealun Skills](https://github.com/tealun/Tealun-Skills) (`planner`, `tasker`, `bugfixer`, `auditor`, `evolver`) and works on its own as well.

**Install:** copy the `labsmith/` folder into your agent's skills directory (for Claude Code `~/.claude/skills/labsmith/`), then start a new session. **Use:** `/labsmith <what you need>` or "use labsmith to …".

License: MIT.
