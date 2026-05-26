---
name: operation-manual-workflow
description: Create training-ready operation manuals from live admin URLs, Axure prototypes, local prototype packages, screenshots, page materials, or business notes. Use when the user asks for 操作手册, 用户手册, 培训手册, 后台系统操作手册, 真实后台链接制册, 管理后台页面采集, Chrome 插件采集, 在线系统操作说明, Axure 原型生成手册, 页面操作说明, 上线培训材料, 保存/提交前确认, field/button/state instructions, exhaustive button/dropdown/checkbox coverage, configuration linkage, or step-by-step business operation documentation.
---

# Operation Manual Workflow

Use this skill to turn live-system or prototype evidence into a maintainable Markdown operation manual for business training, implementation delivery, QA review, or onboarding.

## Role

Act as a product training documentation engineer, senior product manager, and QA validation assistant. The output should help a business user complete operations step by step, while preserving enough page logic for implementation and testing teams to trace the prototype.

## Inputs

Accept one or more of these inputs:

- Axure published URL, localhost URL, or `file:///` URL.
- Live admin URL or business system URL, including login-protected后台, 商管后台, 管理后台, CRM, ERP, OMS, or other operational systems.
- Local Axure package directory containing files such as `start.html`, `index.html`, `resources/`, `data/`, `files/`, or `images/`.
- Screenshots, page exports, page notes, PRD snippets, or business process notes.
- Module name, system side, user role, target reader, output format, page scope, screenshot requirements, or menu hierarchy.

When the user provides a live admin URL or real business system link, read `references/live-admin-manual-workflow.md` before execution. When the user provides an Axure prototype, read `references/axure-manual-workflow.md` before execution. When the user only provides screenshots or notes, use the same manual structure but mark unverifiable interactions as pending confirmation.

## Decision Tree

- **Live admin URL or real business system link**: use `references/live-admin-manual-workflow.md`, collect browser evidence with `@chrome` / Codex Chrome Extension from the user's Chrome session, simulate safe interactions, and stop before any high-risk data change unless the user explicitly authorizes it.
- **Axure URL or local Axure package**: inspect or open the prototype first with `references/axure-manual-workflow.md`, capture pages and interactions, then write the manual.
- **Only screenshots or static page exports**: document visible regions, fields, buttons, and steps; mark hidden transitions, permissions, and validation as `待确认`.
- **Only business notes or PRD text**: create a draft manual structure and pending list; do not claim screenshots or prototype behavior exists.
- **User asks for Word/PDF**: finish Markdown first, then convert only after the Markdown manual is complete and reviewed for obvious gaps.

## Core Principles

- Browse or inspect evidence before writing. Do not produce a manual only from filenames, page titles, or guesses.
- Organize by user operation path, not by internal implementation.
- Use the prototype's terminology exactly when it is visible. Do not rename business terms.
- Treat each page, modal, drawer, or important interactive state as a documentable unit.
- Treat every visible action control as a candidate evidence target: buttons, links, row actions, dropdowns, radio groups, checkboxes, switches, date pickers, steppers, tabs, expand/collapse controls, upload/import/export entries, and copy/duplicate actions.
- Capture dynamic form behavior, especially when a selection changes downstream fields, table columns, matrix rows, validation text, disabled states, or default values.
- Do not invent validation rules, permissions, status meanings, error messages, or irreversible behavior that the prototype or user material does not show.
- On live systems, low-risk interactions such as navigation, filtering, reset, pagination, sorting, tab switch, opening details, and opening edit pages may be simulated directly.
- On live systems, high-risk data changes such as save, submit, delete, approve, publish, import, export sensitive data, batch operation, or status change require explicit user confirmation before execution. If the user explicitly authorizes a scoped high-risk workflow for the current module or test object, execute authorized actions within that scope without asking again for each click; keep actions on temporary/test records when possible and document the exact scope. If confirmation is absent, do not perform the action; document it as `待确认`.
- Mark gaps as `待确认`, especially missing clicks, unclear field constraints, absent errors, login blocks, inaccessible pages, and incomplete states.

## Workflow

1. **Read project rules**
   - In Axhub projects, read `AGENTS.md`, `rules/resource-management-guide.md`, and `rules/documentation-guide.md` when available.
   - Default output paths:
     - Manual: `src/docs/manuals/<模块名称>操作手册.md`
     - Page list: `src/docs/manuals/<模块名称>页面清单.md`
     - Pending items: `src/docs/manuals/<模块名称>待确认项.md`
     - Screenshots: `src/docs/assets/manual/`

2. **Clarify only blocking gaps**
   - Continue directly if the prototype/material, target module, and reader can be inferred.
   - Ask only when the module scope, access method, or output format cannot be derived safely.
   - Minimal clarification: module name, target reader/role, source URL or directory, and whether screenshots are required.

3. **Collect evidence**
   - For live admin URLs or real business system links, follow `references/live-admin-manual-workflow.md`.
   - For Axure URLs or packages, follow `references/axure-manual-workflow.md`.
   - For screenshots or static notes, identify page names, visible regions, fields, buttons, states, and operation paths from the supplied material.
   - Maintain a lightweight evidence map while working: page, source, URL, captured state, interaction action, screenshot filename, risk level, confirmation status, confidence, and pending issue.
   - Build a control coverage inventory before writing: list every visible button, link, row operation, dropdown, checkbox, radio, switch, date control, tab, expandable area, and matrix/table configuration control; mark each as `已实际操作`, `仅打开查看`, `已填写但未提交`, `因权限/风险未执行`, or `待确认`.
   - For configuration forms, deliberately test representative selection changes and validation states: no option selected, one option selected, multiple options selected, option removed, and restoring the safe/default state. Capture before/after evidence when downstream regions change.
   - For copy/duplicate, import/export, batch, status, approval, and delete controls, at least open or document the entry. Execute the persistent result only when authorized and scoped.
   - Record inaccessible pages, broken assets, login requirements, and non-clickable interactions in the pending list.

4. **Create the manual**
   - Use `references/manual-template.md` as the default structure.
   - Keep the manual readable for training: explain entry, action, input, result, limitation, and exception handling.
   - Keep screenshots linked with stable relative paths from the manual.

5. **Create supporting documents**
   - Page list: page name, hierarchy, entry path, page type, status, screenshot reference, and notes.
   - Pending items: page, issue type, issue description, impact, and recommended confirmer.
   - Omit supporting documents only when the user explicitly asks for a single manual and the source is small.

6. **Validate**
   - Apply `references/manual-quality-checklist.md`.
   - Check Markdown headings, tables, screenshot paths, pending items, and whether every core page has operation steps.
   - Check that every asserted rule is backed by visible evidence, user material, or an explicit `待确认` marker.
   - Check the control coverage inventory against the manual and page list; no visible primary button, row action, dropdown, checkbox/radio group, or dynamic matrix/control linkage should be omitted silently.
   - Final response must list generated paths, source coverage, identified pages/interactions, validation result, and pending confirmations.

## Output Style

- Prefer Markdown unless the user explicitly asks for Word or PDF.
- Use direct instructional verbs such as "进入", "点击", "填写", "选择", "保存", "提交", "返回".
- Avoid PRD language that discusses internal system implementation.
- Avoid vague claims like "系统自动处理" unless the prototype or user material shows the result.
- Clearly distinguish "已实际操作", "仅打开查看", "已填写但未提交", and "因高风险未执行".
- Use `待确认` instead of filling unknown business rules.

## Reference Files

- `references/live-admin-manual-workflow.md`: Live admin URL access, `@chrome` plugin capture, logged-in Chrome session reuse, tab claiming/finalization, interaction risk grading, screenshots, and safety confirmations.
- `references/axure-manual-workflow.md`: Axure access, browser sizing, page traversal, screenshots, interactions, and edge cases.
- `references/manual-template.md`: Standard Markdown manual structure and page/function templates.
- `references/manual-quality-checklist.md`: Validation checklist and final response format.
