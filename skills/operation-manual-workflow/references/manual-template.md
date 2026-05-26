# Operation Manual Template

Use this template as the default Markdown structure. Trim sections that do not apply, but do not remove evidence, pending items, or operation steps needed for training.

## File Paths

Default Axhub paths:

```text
src/docs/manuals/<模块名称>操作手册.md
src/docs/manuals/<模块名称>页面清单.md
src/docs/manuals/<模块名称>待确认项.md
src/docs/assets/manual/<截图文件名>.png
```

Screenshot links from files under `src/docs/manuals/` should usually be:

```md
![页面名称](../assets/manual/<截图文件名>.png)
```

## Manual Structure

Please refer to this document template first： `src/docs/templates/functional-module-operation-manual.md` 

When the source contains live pages, Axure interactions, drawers, modals, or secondary pages, include the following delivery sections unless the user explicitly asks for a smaller document.

## Page Operation Coverage Table

```md
## 页面操作覆盖表

| 页面/状态 | 入口 | 页面类型 | 操作风险 | 执行状态 | 截图 | 说明 |
| --- | --- | --- | --- | --- | --- | --- |
| 列表默认态 | 菜单进入 | 列表页 | Low | 已实际操作 | ../assets/manual/xxx.png | 已采集筛选区、工具栏、表格 |
| 新增抽屉 | 新增按钮 | 抽屉 | Medium | 已填写但未提交 | ../assets/manual/xxx.png | 保存属于高风险，未获授权未执行 |
```

Execution status must use one of: `已实际操作`, `仅打开查看`, `已填写但未提交`, `已切换并复原`, `因缺少用户输入被阻断`, `因高风险未执行`, `因权限不足未执行`, `待确认`.

## Secondary Page / Drawer / Modal Template

Use this block for each secondary page, drawer, or modal that is opened or blocked:

```md
### {二级页面/抽屉/弹窗名称}

- 入口路径：{菜单 / 页面 / 按钮 / 行操作 / 更多菜单}
- 打开方式：{点击 xxx / 选择 xxx / 从 xxx 行进入}
- 页面用途：{面向业务用户说明用途}
- 页面类型：{详情页 / 新增页 / 编辑页 / 配置页 / 审核页 / 规则页 / 日志页 / 抽屉 / 弹窗}
- 操作风险：{Low / Medium / High / Blocking Input}
- 执行状态：{已实际操作 / 仅打开查看 / 已填写但未提交 / 已切换并复原 / 因缺少用户输入被阻断 / 因高风险未执行 / 因权限不足未执行 / 待确认}
- 截图：![{名称}](../assets/manual/{截图文件名}.png)

#### 字段说明

| 字段名称 | 控件类型 | 是否必填 | 默认值 | 可选项/示例 | 校验说明 | 备注 |
| --- | --- | --- | --- | --- | --- | --- |

#### 按钮说明

| 按钮名称 | 所在区域 | 点击后结果 | 是否高风险 | 是否已执行 | 备注 |
| --- | --- | --- | --- | --- | --- |

#### 关闭/返回说明

- 关闭方式：{返回 / 关闭图标 / 取消 / 点击遮罩 / 面包屑}
- 是否存在未保存确认：{是 / 否 / 未触发 / 待确认}
- 保存/提交前置条件：{字段、权限、测试对象、用户授权}
- 风险边界：{哪些动作只打开查看，哪些动作未获授权未执行}
- 待确认项：{无 / xxx}
```

## Blocking Input Record

```md
## 阻断类输入记录

| 阻断位置 | 所需字段 | 用户是否已提供 | 后续动作 | 当前状态 | 影响范围 |
| --- | --- | --- | --- | --- | --- |
| 新增抽屉 | 订单号 | 否 | 等待用户提供后继续校验订单带出信息 | 因缺少用户输入被阻断 | 新增流程字段校验与提交前确认 |
```

## High-Risk Action Record

```md
## 高风险动作未执行记录

| 动作 | 影响对象 | 未执行原因 | 需要的用户授权 | 当前已采集证据 |
| --- | --- | --- | --- | --- |
| 保存 | 当前编辑记录 | 未获得明确授权 | 允许保存该测试记录并验证保存结果 | 已打开编辑页、采集字段、底部按钮和保存前状态 |
```

## Page List Template

```md
# {模块名称}页面清单

| 序号 | 页面名称 | 层级 | 入口路径 | 页面类型 | 操作风险 | 执行状态 | 截图 | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
```

## Pending Items Template

```md
# {模块名称}待确认项

| 序号 | 页面 | 问题类型 | 问题描述 | 影响 | 建议确认人 |
| --- | --- | --- | --- | --- | --- |
```
