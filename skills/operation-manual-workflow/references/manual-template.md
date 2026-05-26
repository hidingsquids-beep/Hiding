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

## Page List Template

```md
# {模块名称}页面清单

| 序号 | 页面名称 | 层级 | 入口路径 | 页面类型 | 采集状态 | 截图 | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- |
```

## Pending Items Template

```md
# {模块名称}待确认项

| 序号 | 页面 | 问题类型 | 问题描述 | 影响 | 建议确认人 |
| --- | --- | --- | --- | --- | --- |
```
