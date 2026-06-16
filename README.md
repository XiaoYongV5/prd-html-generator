# prd-html-generator

交互原型生成器：让 claude-skills / Codex / AI Agent 基于产品需求、页面描述、截图或业务场景，一次性生成 **产品经理可交付的页面 PRD** 和 **可离线运行的单文件 HTML 交互原型** 。

该 Skill 适合用于后台管理、业务工作台、CRUD 页面、表单流程、详情页、仪表盘、审批流、配置页等产品页面设计场景。生成结果遵循 `style04` 视觉规范，并内置 PRD 预览抽屉与 PRD-to-Prototype 页面定位能力。


## Demo

### 1. HTML 交互原型效果

![](https://gitee.com/xiao_yong_Zhang/image-bed/raw/master/2026/XiaoYong_2026-06-16_16-31-30.png)

### 2. 右侧 PRD 说明抽屉

![](https://gitee.com/xiao_yong_Zhang/image-bed/raw/master/2026/XiaoYong_2026-06-16_16-32-26.png)

### 3. PRD 条目定位页面区域

![](https://gitee.com/xiao_yong_Zhang/image-bed/raw/master/2026/XiaoYong_2026-06-16_16-33-16.png)



## 它生成了什么

每次生成会输出两类文件：

```text
prd-html-output/
└── YYYYMMDD/
    ├── html/
    │   └── YYYYMMDD-<page-slug>.html
    └── prd/
        └── YYYYMMDD-<page-slug>-prd.md
````

输出内容包括：

- `HTML`：可直接用浏览器打开的单文件交互原型
- `PRD`：结构化 Markdown 页面产品需求文档
- 内置 `PRD 说明` 抽屉
- PRD 条目与页面区域的一一对应定位
- 页面区域高亮、滚动定位、抽屉关闭、ESC 关闭等基础交互
- 真实业务样例数据、状态、按钮、表单、表格、弹窗、抽屉、分页、异常状态等

***

## 核心功能

- **PRD + HTML 一次生成**\
  同步生成页面 PRD 和 HTML 原型，避免文档与原型脱节。
- **style04 视觉规范**\
  默认使用统一的后台产品视觉风格，包括渐变头部、操作区、筛选区、表格、状态标签、弹窗、分页和 PRD 抽屉。
- **单文件 HTML 原型**\
  输出完整离线 HTML 文件，包含内联 CSS 和 JavaScript，不依赖构建工具。
- **动态页面结构识别**\
  根据页面类型生成对应布局，不强行套用固定 CRUD 模板。支持列表管理、表单提交、详情页、仪表盘、配置页、审批流、步骤流程等页面类型。
- **PRD 预览抽屉**\
  原型右侧内置 `PRD 说明` 抽屉，可在页面中直接查看对应 PRD。
- **PRD-to-Prototype 定位**\
  PRD 中的布局项和功能项可以点击定位到 HTML 页面中的对应区域，并触发高亮效果。
- **产品经理友好**\
  输出内容包含页面概述、角色权限、入口前置条件、页面布局、功能设计、业务规则、异常处理、埋点日志、非功能要求、待确认事项和 Checklist。

***

## 使用场景

适合在以下场景中使用：

- 根据一句页面需求生成完整页面原型
- 根据已有 PRD 补充 HTML 交互原型
- 根据截图或页面描述反向整理 PRD
- 为后台管理系统快速设计页面
- 为产品评审准备可点击原型
- 为研发交付页面结构、字段、状态和交互说明
- 为 CRUD、表单、详情、工作台、审批流、配置页生成统一规格文档

示例请求：

```text
使用 prd-html-generator 生成一个机器人地图编辑工作台页面，包含地图画布、图层管理、路径编辑、任务点列表、属性面板和 PRD 说明抽屉。
```

```text
帮我生成一个会员等级配置页面，需要支持筛选、表格、新增、编辑、详情、停用和删除确认。
```

```text
根据这个页面截图生成对应的 PRD 和 style04 HTML 原型。
```

***

## 安装

### Option 1: Install As A Codex Skill

将本仓库放入 Codex skills 目录，例如：

```text
~/.codex/skills/prd-html-generator
```

或 Windows：

```text
C:\Users\<username>\.codex\skills\prd-html-generator
```

目录结构建议如下：

```text
prd-html-generator/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   └── prd_template.md
├── assets/
│   └── style04_template.html
└── README.md
```

安装后，在 Codex 中提出与页面 PRD、HTML 原型、后台管理页面、style04 原型相关的请求时，该 Skill 会被自动触发。

### Option 2: Install As A Trae Skill

如果你使用 Trae，可将 Skill 放入：

```text
C:\Users\<username>\.trae\skills\交互原型生成器-prd-html-generator
```

确保目录内至少包含：

```text
SKILL.md
references/prd_template.md
assets/style04_template.html
```

***

## Repository Structure

```text
prd-html-generator/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
├── assets/
│   └── style04_template.html
├── references/
│   └── prd_template.md
└── docs/
    └── images/
        ├── demo-01-html-prototype.png
        ├── demo-02-prd-drawer.png
        └── demo-03-prd-region-highlight.png
```

说明：

- `SKILL.md`：Skill 的核心指令，定义触发场景、生成流程和质量要求
- `agents/openai.yaml`：Skill 在工具界面中的展示信息
- `references/prd_template.md`：PRD 输出模板
- `assets/style04_template.html`：HTML 原型视觉和交互模板
- `docs/images/`：README 演示图目录

***

## Output Convention

默认输出目录：

```text
prd-html-output/YYYYMMDD/html/YYYYMMDD-<page-slug>.html
prd-html-output/YYYYMMDD/prd/YYYYMMDD-<page-slug>-prd.md
```

例如：

```text
prd-html-output/20260616/html/20260616-robot-map-editor-workbench.html
prd-html-output/20260616/prd/20260616-robot-map-editor-workbench-prd.md
```

***

## Generated PRD Structure

生成的 Markdown PRD 通常包含以下章节：

```text
文档基本信息
一、页面概述
二、页面布局
三、功能详细设计
四、业务规则
五、异常处理
六、埋点与日志
七、非功能要求
八、待确认事项
附录 Checklist
```

其中“页面布局”和“功能详细设计”会根据实际页面动态生成，不会强行套用固定模块。

***

## Generated HTML Requirements

生成的 HTML 原型具备以下特征：

- 完整 `<!DOCTYPE html>` 单文件
- 默认中文界面文案
- 内联 CSS 和 JavaScript
- 可直接用浏览器打开
- 包含真实样例数据
- 包含页面所需的交互状态
- 包含 `PRD 说明` 按钮
- 包含右侧 `#prd-drawer`
- 支持 PRD 条目点击定位页面区域
- 支持交互区域整体高亮
- 支持关闭按钮、遮罩点击、ESC 关闭抽屉

***

## style04 Visual Language

默认视觉风格包括：

| Element          | Style                 |
| :--------------- | :-------------------- |
| Header           | 渐变背景，居中白色标题，右侧 PRD 按钮 |
| Primary Button   | `#ff6b6b`，白字，6px 圆角   |
| Secondary Button | 浅灰背景，灰色边框，6px 圆角      |
| Filter Section   | `#f8f9fa` 背景，8px 圆角   |
| Table            | 白色背景，行 hover 高亮       |
| Status Badge     | Pill 胶囊样式，不同状态不同颜色    |
| Pagination       | 左侧总数，右侧页码             |
| Modal            | 白色面板，半透明遮罩            |
| PRD Drawer       | 右侧滑入，渐变头部，可滚动         |
| Region Highlight | 虚线描边与呼吸高亮             |

***

## Quality Checklist

生成结果应满足：

- PRD 与 HTML 页面结构一一对应
- 没有未替换的 `{{placeholder}}`
- HTML 可离线打开
- PRD 抽屉可打开、滚动、关闭
- ESC 和遮罩点击可关闭抽屉
- 每个重要页面区域都有 `data-prd-target`
- PRD 中可点击定位对应页面区域
- 高亮交互清晰可见
- 页面缺失的模块不会被写进 PRD
- 表格、筛选、分页、弹窗、抽屉、表单、状态与业务规则保持一致

***

## Example Prompt

```text
使用 prd-html-generator 生成一个企业客户管理页面。

页面需要包含：
- 顶部概览指标
- 客户名称、客户等级、跟进状态筛选
- 客户列表表格
- 新增客户弹窗
- 编辑客户弹窗
- 客户详情抽屉
- 删除确认
- 分页
- PRD 说明抽屉
- PRD 条目点击定位页面区域

请同时输出 HTML 原型和 Markdown PRD。
```

***

## Notes

这个 Skill 不是通用网页生成器，而是面向产品设计和需求交付的页面原型生成器。它的重点是让 PRD、页面结构、字段、状态、交互、业务规则和可视化原型保持一致。

如果只是生成营销落地页、品牌官网或自由风格网页，建议使用其他更适合视觉创意类页面的前端生成 Skill。

***

## Star History

<a href="https://www.star-history.com/?repos=XiaoYongV5%2Fprd-html-generator&type=timeline&logscale=&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=XiaoYongV5/prd-html-generator&type=timeline&theme=dark&logscale&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=XiaoYongV5/prd-html-generator&type=timeline&logscale&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=XiaoYongV5/prd-html-generator&type=timeline&logscale&legend=top-left" />
 </picture>
</a>
