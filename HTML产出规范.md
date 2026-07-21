# HTML 产出规范

> 调研类任务的**交互式 HTML 报告**产出规范。把本文件 `@` 引用给 Claude，即可复用这套形态、流程与设计系统。
> 参考实现：[`类Claude-Code框架调研.html`](./类Claude-Code框架调研.html)、[`WorkBuddy类Agent调研.html`](./WorkBuddy类Agent调研.html)。

---

## 1. 交付形态（硬约束）

- **单个自包含 `.html`**：所有 CSS / JS / SVG 全部**内联**，双击用浏览器即开，**纯本地、零外部依赖、不联网**。
- **绝不引外部 webfont / CDN / 图床**：离线或 CSP 下会静默 fallback。字体一律用**系统字体栈**（见 §4）。
- **落地方式**：写进 git 项目目录（**不**发布 claude.ai Artifact 托管页）。
- **语言**：中文正文 + 保留英文技术术语（`agent loop`、`tool_use`、`SWE-bench` 等原样）。
- **图表 / 图形**：用内联 SVG 或 Canvas 手绘，不外链。

## 2. 构建流程（最关键：规避 API stall）

> 教训：一次性 `Write` 整个大文件会导致响应中途 stall。

- **先 `Write` 骨架**：`<title>` + 完整 `<style>`（整套设计 token 与组件类）+ 一个唯一占位符注释（如 `<!--SECTIONS-->`）。
- **再用 `Edit` 分块追加**：每次把占位符替换成「本段内容 + 新占位符」，逐节推进，每块中等体量。
- **注意 `Edit` 锚点**：中文全角逗号 `，` vs 半角 `,`、全角/半角括号极易不匹配导致锚点失败——**全篇统一标点**；失败时先 `grep` 取文件里的精确原文再改。
- **结尾 `grep` 校验**：标签平衡（`<section>`/`</section>`、`<details>`/`</details>`、`<pre>`/`</pre>`）、占位符清零、无游离 `</div>`。

## 3. 数据来源（内容真实、可核实）

- 用 **workflow 多智能体联网调研** → 每个智能体按 **JSON schema 结构化输出** → 读回 output 文件 → 填充页面。
- 结构化 schema 字段建议：`是什么 / 怎么工作 / 深入机制 / 设计权衡 / 具体实现 / 常见陷阱 / 关键数字`。
- 要「深」就跑**第二轮 workflow** 专攻遗漏 / 深挖主题，`effort: high` + 要求 implementation-grade。
- 页脚**必须标时效性 + 核实提醒**（数字会过时，决策前回官方源）。

## 4. 设计系统（先规划，再编码）

- 先加载 **`artifact-design` skill**，按它定 3 件事：**配色（4–6 个具名 hex）、字体（2–3 个角色）、布局概念（一两句）**。
- **主题母题从主题世界提取**（例：编码 agent 用 agent loop 回路 + 琥珀 CRT 蓝图），不要通用装饰。
- **主动避开 AI 套路**：荧光绿终端、暖米色 `#F4F1EA` + 衬线、紫蓝渐变 hero、Inter / Space Grotesk、emoji 当分节符、全居中、到处 `rounded-lg`。
- **字体（零 webfont）**：标题 / 正文用系统栈含 CJK（`-apple-system, "PingFang SC", "Microsoft YaHei"...`）；等宽体作「标识声音」（标签 / 代码 / 数字），但**严格只包 Latin / 数字**，含中文会 CJK fallback 错位。
- **CSS 规范**：用 `:root` CSS 变量做 token；间距靠 flex/grid `gap`（不用 margin 叠加）；盯 selector specificity 别互相抵消；`font-variant-numeric: tabular-nums` 对齐数字。

## 5. 结构与交互

- **吸顶 TOC + 滚动高亮**（`IntersectionObserver` scroll-spy）。
- **可展开 `<details>` 面板** + **Tab 切换**（`aria-selected` + `hidden`）；纯 vanilla JS，内联。
- **编号只用在真序列**上（路线图阶段、循环步骤、时间线）；非序列内容（如并列组件）用**具名标签**而非 `01 / 02`。
- **无障碍 / 健壮**：`:focus-visible` 可见焦点；`prefers-reduced-motion` 关动画；宽内容（表格 / 代码 / 图）套 `overflow-x:auto`，页面 body 永不横向滚动。
- **响应式**：相对单位 + grid/flex `gap` + `max-width:100%`；正文行宽 ~65–72ch。

## 6. 内容组织范式（可复用的「卡片语法」）

每个复杂主题统一用这套块：

**是什么 → 怎么工作 → 深入机制（深色块，线级细节）→ 核心概念（术语网格）→ 设计权衡（左边框决策块）→ 常见陷阱（✕ 红清单）→ 关键数字（mono chips）→ 真实实现**

视觉组件：卡片、术语双列网格、决策块、风险 / 成功清单（✕/✓）、数字 chip、深色代码 `<pre>`。

## 7. 收尾与交付

- `grep` 结构校验 → 覆盖写进项目 → 更新 `README.md`（指向链接 + 一句时效提醒）→ `commit`（push 前**确认**，属外部动作）。
- 多份文档共存时：**互不混淆**，`README.md` 分节列出，每次只改目标文件。

---

## 附：一次任务的标准推进顺序

1. 明确交付形态与深度（必要时用 `AskUserQuestion` 问 2 个关键点：呈现形式、构建深度）。
2. 加载 `artifact-design`，启动调研 workflow（后台跑）。
3. 等 workflow 完成 → 读回结构化数据。
4. `Write` 骨架（`<title>` + `<style>` + 占位符）。
5. `Edit` 分块填充各章节。
6. `grep` 校验结构 → 覆盖进项目。
7. 更新 `README.md` → `commit` →（确认后）`push`。
