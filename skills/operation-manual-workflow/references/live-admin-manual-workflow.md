# Live Admin Manual Workflow

Use this reference when an operation manual is based on a real后台 URL, live admin page, CRM/ERP/OMS page, or any online business system where user data may be visible or changeable.

## Startup Rules

1. Normalize obvious URL typos before access, such as `hhttps://` to `https://`.
2. Prefer `@chrome` / Codex Chrome Extension for live admin collection so the workflow can reuse the user's real Chrome login state, open tabs, cookies, extensions, and permission context.
3. First try to claim an already-open target tab that matches the requested URL or module. If no suitable tab exists, open a new Chrome tab with the normalized URL.
4. Do not depend on a DevTools debugging port and do not install a temporary browser-control layer by default.
5. If Chrome plugin communication fails, stop and explain the likely blocker instead of silently switching to another browser solution. Common blockers include Chrome not running, Codex Chrome Extension not installed or disabled, native host failure, extension permission issues, or plugin communication timeout.
6. Use only the user-provided or already-available authenticated session. Do not bypass authentication, scrape credentials, or perform permission escalation.
7. If the live system cannot be accessed, capture or describe the blocker page when safe and list the blocker in pending items.

## Chrome Plugin Control Pattern

Use the `@chrome` plugin as the default control layer for live admin pages.

Recommended sequence:

1. Name the browser session for traceability, for example `operation manual - <module>`.
2. Inspect existing Chrome tabs and claim the tab whose URL, title, or visible page text matches the requested module.
3. If no matching tab exists, open a new tab and navigate to the normalized URL.
4. Wait for the page to settle, then record the final URL, title, login/permission state, visible page heading, menu path, and primary text.
5. Capture a screenshot and DOM/text snapshot before interactions.
6. Execute low-risk interactions through the claimed tab: query, reset, pagination, sorting, tab switch, detail view, drawer/modal open, and edit-entry open without saving.
7. For medium-risk interactions, fill or preview only when no persistent result is triggered; document that no save/submit happened and restore values when practical.
8. Before any high-risk operation, pause and ask the user for explicit authorization unless a scoped authorization already covers that exact module, object, and action.
9. Save evidence into the evidence map after each meaningful state change.
10. At the end, release or close tabs with `browser.tabs.finalize({ keep })`. Keep the final deliverable tab when it helps user review; close or release intermediate tabs.

If the user explicitly asks to avoid fallback tools, honor that instruction. If Chrome is unavailable, report the exact Chrome/plugin blocker and stop after documenting what could not be collected.

## Interaction Risk Levels

Classify every interaction before executing it.

| Risk | Examples | Execution rule |
| --- | --- | --- |
| Low | menu navigation, filter, reset, pagination, sorting, expand/collapse, tab switch, view detail, open drawer, open edit page without saving | May execute directly for evidence collection. |
| Medium | fill form without submitting, preview upload, open export dialog without confirming, switch a control then revert before save, select/deselect checkboxes to observe downstream matrix changes | May execute only when it does not persist data; document that no save/submit happened. |
| High | save, submit, delete, approve, reject, publish, unpublish, import, export sensitive data, copy/duplicate that creates data, batch operation, status change, send notification, irreversible workflow transition | Stop and ask the user for explicit confirmation before execution unless the user has already granted a scoped authorization for this module/test object. If not confirmed, do not execute and mark `待确认`. |

The confirmation question must name the exact action and affected object when visible, for example:

```text
我已打开“编辑销售商”页面。点击“保存”会修改真实后台数据。是否允许我保存这条记录用于验证保存结果？
```

Do not treat vague approval as permission for all future high-risk actions. If the user gives broad but explicit scoped authorization, record it in the evidence map, for example `confirmed: all high-risk actions on temporary test strategy only`. Stay inside that scope and prefer a clearly named temporary/test record. Ask again only when the action would affect a different object, real production-like data, a different module, or a materially broader risk category.

## Evidence Collection

Collect enough evidence before writing:

| Field | Meaning |
| --- | --- |
| Page | Visible page name or inferred module name |
| URL | Current browser URL after redirects |
| Account state | Logged in, login blocked, permission blocked, or unknown |
| Capture method | `@chrome` claimed existing tab, `@chrome` opened new tab, or blocked by Chrome/plugin issue |
| Interaction action | Clicked, opened, filled, selected, searched, reset, or skipped |
| Visible feedback | New page, drawer, modal, toast, validation text, table refresh, or no visible change |
| Screenshot | Stable screenshot filename |
| Risk level | Low, Medium, High |
| User confirmation | Not needed, confirmed, denied, or not asked because action was skipped |
| Tab result | Kept as deliverable, released, closed, or unavailable |
| Pending item | Missing permission, unclear rule, unexecuted high-risk action, or inaccessible state |

Use `High` confidence only for behavior directly observed in the live page. Use `Medium` for visible but untested controls. Use `Pending` for blocked, skipped, or high-risk behavior without confirmation.

## Control Coverage Inventory

Before writing, create a compact inventory for every important visible control in the selected module. Do not rely only on the main happy path.

| Control group | What to test or document |
| --- | --- |
| Toolbar buttons | Query, reset, add, edit, copy, delete, approve/reject, submit/save, import/export, batch actions, status changes, priority/order changes |
| Row actions | Detail, edit, copy, enable/disable, logs, delete, workflow actions |
| Form controls | Required text fields, dropdowns, radio groups, checkbox groups, switches, date ranges, numeric steppers, uploads, cascaders |
| Dynamic regions | Tables, matrices, calculated fields, validation messages, disabled states, empty states, default values, and columns/rows shown after selections |
| Confirmation layers | Toasts, popovers, modal confirmations, drawer footers, validation text, and secondary confirmations |

For each control, record one of these statuses: `已实际操作`, `仅打开查看`, `已填写但未提交`, `已切换并复原`, `因权限/风险未执行`, or `待确认`.

When a form has dependent configuration regions, capture representative before/after states:

1. Required selection missing, including validation text and downstream empty/disabled regions.
2. One option selected, including rows/columns/matrix entries that appear.
3. Multiple options selected, including how rows/columns expand.
4. Option removed, including how downstream rows/columns shrink or become empty.
5. Restored state before closing or submitting, when practical.

## Recommended Flow

1. Read project rules and confirm output paths.
2. Normalize the URL and open it through `@chrome` / Codex Chrome Extension by claiming an existing matching Chrome tab first, or opening a new Chrome tab when needed.
3. Capture the initial page, final URL after redirects, title, major visible text, screenshot, DOM/text snapshot, and login/permission state.
4. Identify the requested module and page scope from URL, breadcrumbs, menus, headings, or user instruction.
5. Execute low-risk interactions to capture states:
   - search/filter and reset
   - table pagination/sorting
   - row detail or drawer
   - tabs and expandable areas
   - edit page entry without saving
   - validation states only when they can be produced without persistence
6. Build the control coverage inventory and exercise representative controls:
   - open every primary toolbar action at least to its safe boundary
   - test dropdown/radio/checkbox/switch/date changes that visibly affect the page
   - capture validation states for missing required selections when safe
   - capture dynamic matrix/table changes caused by configuration range changes
   - include copy/duplicate flows; execute creation only when authorized
7. For medium-risk interactions, fill or preview only when no persistence occurs; restore the page when practical.
8. Before any high-risk action, stop and ask the user unless scoped authorization is already explicit. If the user does not confirm, document the intended path and mark it `待确认`.
9. Write manual, page list, and pending items from the evidence map and control coverage inventory.
10. Validate screenshot paths and explicitly report which interactions were executed, skipped, or blocked.
11. Finalize Chrome tabs with `browser.tabs.finalize({ keep })`; report whether the deliverable tab was kept and whether intermediate tabs were released or closed.

## Screenshot Rules

Default directory:

```text
src/docs/assets/manual/
```

Live admin screenshot filename format:

```text
<模块名称>_<序号>_<页面或动作>.png
```

Examples:

```text
销售商商家管理列表_01_初始访问.png
销售商商家管理列表_02_筛选条件展开.png
销售商商家管理列表_03_详情抽屉.png
销售商商家管理列表_04_编辑页未保存.png
```

For sensitive data, mask or avoid exposing secrets where practical. If screenshots may contain real personal, financial, credential, or commercial data, mention the sensitivity in the final response and pending items.

## Failure And Boundary Handling

- **URL redirects to login**: capture login page, record final URL, do not invent business page behavior.
- **Chrome plugin unavailable**: stop and report the blocker. Do not automatically switch to another browser automation method unless the user explicitly requests a fallback.
- **Chrome extension communication fails**: state whether the likely cause is Chrome not running, Codex Chrome Extension missing/disabled, native host failure, extension permissions, or timeout; keep the manual limited to supplied screenshots/notes only if the user accepts reduced evidence.
- **User is logged in only in Chrome**: prefer claiming the already-open Chrome tab so the manual uses the actual authenticated session.
- **Permission denied**: capture the denial page and mark missing role/permission as `待确认`.
- **Page loads but data is empty**: document empty-state behavior only; do not invent list operations.
- **Control is hidden or disabled**: record disabled reason if visible; otherwise mark the rule pending.
- **High-risk action not authorized**: describe the visible entry and expected manual step, but clearly mark the result as unverified.
- **Authorized high-risk workflow**: keep the operation inside the named scope, prefer temporary records, capture confirmations and final state, and clean up temporary data when cleanup itself is authorized and safe.
- **Dynamic form linkage is unclear**: capture the control before/after state and describe only the observed change, for example `未选择商户类型时矩阵显示暂无数据`; list the underlying business rule as `待确认`.
- **Export/import involves real data**: treat as high risk. Open the dialog only if safe; do not confirm export/import without explicit permission.
