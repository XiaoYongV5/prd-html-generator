---
name: prd-html-generator
description: Generate product-manager-ready page PRDs and matching style04 single-file HTML prototypes. Use when Codex needs to write a page PRD, output an HTML prototype, generate a dynamic page layout description from an uploaded or generated page, create a management/admin/workflow/form/detail/dashboard/CRUD page, follow the style04 visual specification, produce a prototype with a right-side PRD preview drawer, or make PRD entries click-highlight the corresponding page regions.
---

# PRD + HTML Prototype Generator

## Core Promise

Generate two aligned deliverables for a product page:

- A standalone HTML prototype using `assets/style04_template.html`.
- A Markdown PRD using `references/prd_template.md`.

Unless the user specifies a location, save files under the current working directory:

- `prd-html-output/YYYYMMDD/html/YYYYMMDD-<page-slug>.html`
- `prd-html-output/YYYYMMDD/prd/YYYYMMDD-<page-slug>-prd.md`

Always tell the user the exact saved paths.

## Required References

Before generating:

1. Read `references/prd_template.md` when drafting the PRD.
2. Read `assets/style04_template.html` when drafting the HTML prototype.

Treat these files as required structure, not optional inspiration.
For HTML, treat `assets/style04_template.html` as a style04 component library and visual baseline. Reuse its CSS tokens, spacing, buttons, tables, cards, modals, drawer behavior, and interaction patterns, but remove or replace template sections that are absent from the requested page.

## Input Handling

Use the user's page name, scenario, screenshots, field lists, statuses, roles, and constraints as source material. If key information is missing but the business domain is clear, make conservative product assumptions and document them in the PRD. Ask a short clarification only when the page name or product domain is impossible to infer.

Extract or infer:

- Page name, English subtitle, and module/domain.
- User roles and permission boundaries.
- Entry points and preconditions.
- Actual page regions and components, such as header, action bar, filters, forms, cards, steps, tabs, charts, tables, lists, detail panels, drawers, modals, pagination, empty states, and preview panels.
- Only the fields/actions that exist in the actual page: filter fields, form fields, table columns, card fields, row actions, bulk actions, pagination, detail fields, delete rules, status values, workflow steps, or chart metrics.
- PRD-to-page navigation targets for each meaningful interactive region and functional module.
- Business rules, abnormal states, analytics events, non-functional requirements, and open questions.

## Generation Workflow

1. Draft a compact page model: page type, actual layout regions, roles, fields, statuses, actions, data objects, and page states.
2. Generate the PRD from `references/prd_template.md`; keep all sections in order and replace every placeholder with real content.
3. Generate the HTML prototype from `assets/style04_template.html`; keep the style04 visual language, replace every placeholder with real content, and remove or replace regions that do not fit the actual page.
4. Embed the PRD content into the HTML drawer so the prototype can preview the same PRD directly.
5. Ensure the PRD and HTML are one-to-one: every visible region, field, operation, status, chart/list/table/card item, modal/drawer, and business rule must match.
6. Add PRD-to-prototype navigation: PRD entries must be able to highlight and scroll to the corresponding page region.
7. Save both files and report paths.

## Dynamic Layout Rules

Build the PRD layout chapter from the actual page, not from a fixed management-page assumption.

- First classify the page type: list management, form submission, detail view, dashboard, wizard/step flow, configuration page, kanban/list, calendar/schedule, document/editor, approval workflow, or mixed page.
- In PRD section `二、页面布局`, include only regions that actually appear in the uploaded/generated page.
- Do not invent `筛选条件区`, `数据表格区`, `分页器`, `新增/编辑弹窗`, `详情弹窗`, or `删除确认` when the page does not have them.
- If a common region is absent, omit it entirely; do not write rows like `无` or `不涉及`.
- Generate the ASCII layout diagram from the final HTML structure after deciding which regions exist.
- In PRD section `三、功能详细设计`, create subsections dynamically for the actual functional modules. Use names such as `3.1 顶部概览`, `3.2 表单填写`, `3.3 指标卡片`, `3.4 趋势图表`, `3.5 数据表格`, `3.6 审批操作`, or `3.7 PRD 说明抽屉` as appropriate.
- Only include table-column definitions when a real table exists.
- Only include filter-query rules when a real filter area exists.
- Only include pagination rules when pagination exists.
- Only include modal rules when the prototype has modals; use drawer/detail-panel rules when those are the actual interaction container.
- Keep stable required PRD chapters: `文档基本信息`, `一、页面概述`, `二、页面布局`, `三、功能详细设计`, `四、业务规则`, `五、异常处理`, `六、埋点与日志`, `七、非功能要求`, `八、待确认事项`, `附录 Checklist`.

## HTML Prototype Rules

The HTML file must be a complete offline single file:

- Include `<!DOCTYPE html>`, `<html lang="zh-CN">`, UTF-8 meta, viewport meta, `<title>`.
- Use inline CSS and JavaScript only unless the user explicitly requests a framework.
- Use Chinese UI copy by default.
- Include realistic sample data, empty state, loading state, and error toast or inline error behavior where relevant.
- Include only interactions required by the page type. For management pages, include filtering, reset, pagination, add/edit modal, detail modal, delete confirmation, and status badges when those functions exist. For non-management pages, build the expected native interactions instead, such as form validation, tab switching, step navigation, chart filtering, drawer preview, card expansion, approval actions, or inline editing.
- Keep the first screen as the actual working prototype, not a landing page.

Required PRD preview interaction:

- Add a `PRD 说明` button in the header right side.
- Implement `openPrdDrawer()`, `closePrdDrawer()`, and `generatePrdContent()`.
- Show a right-side drawer named `#prd-drawer`, with overlay dimming the prototype behind it.
- Drawer width: `min(600px, 100vw)` on desktop/mobile.
- Support internal scroll, close button, overlay click, and `Escape` close.
- The drawer content must match the saved PRD, using HTML rendering of the same sections.

Required PRD-to-prototype navigation:

- Add `data-prd-target="<stable-id>"` to every meaningful interactive page region, such as filters, form sections, action bars, tables, charts, cards, tabs, modals, drawers, approval actions, and step panels.
- Use stable kebab-case target IDs, for example `page-header`, `action-bar`, `filter-section`, `table-list`, `create-edit-modal`, `metric-cards`, `approval-actions`, `step-basic-info`.
- Add `data-prd-label="<Chinese region name>"` to each target so the highlight system can display or debug region identity.
- Implement `toggleInteractiveHighlights()`, `focusPrdTarget(targetId)`, and `clearPrdTargetHighlight()`.
- Provide a visible one-click control named `高亮交互区` in the prototype header or PRD drawer. It toggles all `[data-prd-target]` regions with a dashed outline.
- In `generatePrdContent()`, render each layout/function entry with a clickable control such as `<button class="prd-region-link" onclick="focusPrdTarget('filter-section')">定位筛选区</button>`.
- When a PRD entry is clicked, scroll the matching page region into view and apply a breathing glow around it for at least 2 seconds.
- If the target is inside a hidden modal/drawer/tab/step, open or switch to the required container first, then highlight the target.
- If a target cannot be found, show a toast explaining that the page region is unavailable in the current prototype.
- Do not add navigation links for PRD sections that do not correspond to a visible or triggerable page region.

## style04 Visual Rules

Reuse the visual style from `assets/style04_template.html`:

| Element | Required style |
| --- | --- |
| Header | Gradient background, centered white title, right-side `PRD 说明` button |
| Action bar | Flex layout, primary actions left and secondary actions right, 24px gap |
| Primary button | `#ff6b6b`, white text, 6px radius, hover lift and shadow |
| Secondary button | `#f8f9fa`, gray border, 6px radius |
| Filter section | `#f8f9fa` background, 8px radius, `1px solid #e9ecef` |
| Table | White background, 8px radius, hover row `#f8f9fa` |
| Status badge | Pill shape, 20px radius, status-specific colors |
| Pagination | Total count left, page buttons right, current page `#ff6b6b` |
| Modal | White panel, 8px radius, `rgba(0,0,0,0.5)` overlay |
| PRD drawer | Right slide-in, gradient header, scrollable content, close/overlay/ESC |
| PRD locator | Link-style pill button, `#5352ed` text, subtle lavender background, hover lift |
| Region highlight | Dashed `#5352ed` outline for all interactive regions; breathing `#ff6b6b` glow for focused region |

Default status badge mapping:

| Status | CSS class | Color |
| --- | --- | --- |
| 编辑中 | `.status-editing` | `#e9ecef` background, `#6c757d` text |
| 未生效 | `.status-not-active` | `#cce5ff` background, `#0066cc` text |
| 生效中 | `.status-active` | `#d4edda` background, `#155724` text |
| 已过期 | `.status-expired` | `#fff3cd` background, `#856404` text |
| 已作废 | `.status-invalid` | `#f8d7da` background, `#721c24` text |

## PRD Rules

The Markdown PRD must:

- Follow `references/prd_template.md` in chapter order, while making section `二、页面布局` and section `三、功能详细设计` dynamically reflect the actual page.
- Replace all `{{placeholder}}` tokens with real content.
- Treat template lines such as `生成说明`, `3.X 可选模块写法参考`, and example module lists as authoring guidance only; omit them from the final PRD.
- Include an ASCII layout diagram that matches the HTML.
- Describe each visible HTML region in the matching PRD chapter.
- Include business rules, abnormal handling, analytics/logging, non-functional requirements, open questions, and checklist sections.
- Keep assumptions explicit and testable.

## Delivery Format

When returning inline content instead of writing files, output HTML first, then a divider line `---`, then the PRD.

When writing files, respond with:

- HTML file path.
- PRD file path.
- Brief verification notes.

## Quality Checklist

Before finishing, verify:

- No unreplaced `{{...}}` placeholders remain in either deliverable.
- HTML contains the `PRD 说明` button and `#prd-drawer`.
- `generatePrdContent()` content matches the saved PRD.
- Drawer opens, scrolls, and closes by close button, overlay click, and ESC.
- HTML contains `toggleInteractiveHighlights()`, `focusPrdTarget(targetId)`, and `clearPrdTargetHighlight()`.
- Every meaningful interactive region has `data-prd-target` and `data-prd-label`.
- PRD drawer contains clickable locator controls for corresponding page regions.
- Clicking a PRD locator scrolls to the region and shows a breathing highlight.
- The `高亮交互区` control toggles all interactive region outlines.
- Every actual page region appears in PRD section `二、页面布局`; absent regions are not documented.
- Functional subsections in PRD section `三、功能详细设计` match the actual prototype modules.
- If filters exist, filter fields and query behavior match the PRD.
- If tables exist, table columns and row operations match the PRD.
- If forms, modals, drawers, tabs, charts, cards, steps, or pagination exist, their behavior matches the PRD.
- Status values and badge classes match the PRD and CSS when the page has statuses.
- UI colors, spacing, and actual components follow style04.
- The final response tells the user both saved paths.
