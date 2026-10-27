# 批量编辑功能点设计模板

## 适用场景
- 用于批量修改、批量更新、勾选后统一赋值、批量编辑字段等需求。
- 适用于用户、设备、资产、策略、对象组、标签、任务配置等具备列表勾选能力且需要对多个对象统一修改同一字段的页面。
- 当批量编辑仅涉及少量字段且赋值规则简单时，使用单弹窗表单承载；当字段多、依赖关系复杂或需要分步配置时，评估是否升级为抽屉或页面承载。

## 推荐页面组合
| 页面ID | 页面名称 | 页面类型 | 页面用途 | 入口方式 | 关键交互 |
|---|---|---|---|---|---|
| batch-edit | 批量编辑 | 表单弹窗 | 勾选需要统一修改的字段并赋值，对已勾选对象执行批量更新 | 来源列表页勾选数据后点击“批量编辑” | 勾选字段、统一赋值、确定提交、结果反馈 |

## 推荐流程
```text
来源列表页（勾选对象）
  点击“批量编辑”
    批量编辑弹窗
      勾选要编辑的字段
      统一修改控件内容
      点击“确定”
      提交校验
        成功 / 部分失败反馈
        刷新列表并清除勾选
```

## 功能契约

- 本契约为机器可读的结构断言，用于校验批量编辑设计的结构完整性；结构与组件断言均不可调整。
- 防误读关键句：推荐页面组合表与下方线框中的"字段"是**一个配置项内的一行**（复选框 + 字段名 + 控件同行），不是多个独立配置项；可编辑字段清单由业务确定，属 `tunable`，字段增减不改变行结构。

```json
{
  "feature": "batch-edit",
  "nodes": [
    {
      "featurePageId": "batch-edit",
      "containerType": "modal",
      "wireframeRef": "references/04-feature/batch-edit.md#batch-edit-wireframe",
      "requiredAsciiTokens": ["批量编辑", "已勾选", "确定", "取消"],
      "rowSignature": {
        "columnsPerRow": ["字段复选框", "字段名", "值控件"],
        "sameRow": true
      },
      "requiredComponents": ["IxProFormModal", "IxCheckbox"]
    }
  ],
  "edges": [
    { "from": "source-list", "to": "batch-edit", "label": "批量编辑", "when": "已勾选至少一个对象，否则入口按钮禁用" },
    { "from": "batch-edit", "to": "batch-result", "label": "确定", "variants": ["all-success", "partial-fail", "all-fail"] },
    { "from": "batch-result", "to": "batch-edit", "label": "修正后重试", "when": "variants:all-fail，弹窗保留已填写值" }
  ],
  "linkage": {
    "checkToggle": "未勾选字段的控件保持禁用，勾选后激活，允许一次勾选多个字段",
    "noSelectionGuard": "未勾选任何字段时确定按钮禁用或提交时提示请先勾选要编辑的字段"
  },
  "feedback": {
    "variants": [
      { "id": "all-success", "required": ["成功数量提示", "关闭弹窗", "刷新列表", "清除勾选"] },
      { "id": "partial-fail", "required": ["成功X个失败Y个提示", "失败对象与原因"] },
      { "id": "all-fail", "required": ["失败提示", "失败对象与原因", "保留弹窗内容与已填写值以便修正重试"] }
    ]
  },
  "constraints": {
    "singleModal": true,
    "sameRowFieldAndControl": true,
    "selectionCountHint": "弹窗顶部展示已勾选 X 项",
    "refreshAndClearSelection": "仅全部成功后刷新列表并清除勾选",
    "loadingOnSubmit": true,
    "commonFieldsOnly": "勾选对象字段集不一致时，仅展示所有对象均支持的公共字段"
  },
  "tunable": [
    "editableFields（可批量编辑的字段清单由业务确定）",
    "fieldControlMapping（字段控件按 form-patterns 常见字段组件映射速查适配）"
  ],
  "forbiddenStructures": [
    { "type": "splitFieldControlColumns", "desc": "字段名与赋值控件分离为两列或两步，破坏复选框+字段名+控件的同行结构" },
    { "type": "multiConfigSplit", "desc": "把字段清单拆成多个独立配置项（每字段一个带Label的独立表单项分组）" },
    { "type": "noCheckLinkage", "desc": "字段复选框与控件禁用态不联动" },
    { "type": "omitFeedbackVariants", "desc": "缺少成功/部分失败/全部失败三变体反馈" },
    { "type": "clearSelectionOnFail", "desc": "未全部成功即清除勾选（全部失败时还须保留弹窗已填写值）" }
  ]
}
```

## 页面结构

- 页面类型：表单弹窗
- 页面类型 ID：`form-modal`

#### 页面结构：batch-edit {#batch-edit-wireframe}

```text
┌──────────────────────────────────────────────┐
│ 批量编辑                          [关闭]      │
├──────────────────────────────────────────────┤
│  已勾选 5 项                                   │
│                                              │
│  ☐ 名称        [输入框             ]          │
│  ☐ 所属分组    [下拉选择框         ]          │
│  ☐ 启用状态    (•)启用  ( )禁用               │
│  ☐ 标签        [多选选择框         ]          │
│  ☐ 描述        [多行输入框         ]          │
│                                              │
│                              [确定] [取消]    │
└──────────────────────────────────────────────┘
```

## 组件信息
- 表单弹窗：使用 `IxProFormModal`。
- 字段勾选：使用 `IxCheckbox`，置于每行字段名前。
- 字段控件：按字段类型映射，规则与 `../03-patterns/02-solution-patterns/form-patterns.md` 常见字段组件映射速查一致：单行输入 `IxInput`、多行输入 `IxProTextarea`、单选 `IxRadioGroup`、多选 `IxCheckboxGroup`、下拉单选 `IxSelect`、下拉多选 `IxSelect multiple`、数值 `IxInputNumber`、日期 `IxDatePicker` 等。

## 页面设计要点

### 1. 批量编辑弹窗
- 页面名称：批量编辑。
- 页面ID：`batch-edit`。
- 页面类型：表单弹窗。
- 页面用途：用户勾选需要统一修改的字段并设置新值，提交后对所有已勾选对象批量更新对应字段。
- 页面入口：来源列表页勾选对象后点击“批量编辑”。

#### 页面内容
- 弹窗标题：“批量编辑”。
- 已勾选数量提示：弹窗顶部展示“已勾选 X 项”，X 为当前表格已勾选对象数量。
- 可编辑字段列表：逐行列出当前业务对象可批量编辑的字段，每行由“复选框 + 字段名 + 控件”组成。
- 底部操作：“确定”提交批量更新，“取消”关闭弹窗。

#### 关键交互
- 字段勾选：
  - 未勾选任何字段时，对应字段的控件保持禁用或不可编辑状态。
  - 勾选字段复选框后，该行控件激活，允许输入或选择新值。
  - 支持一次勾选多个字段，统一赋值后一并提交。
- 赋值与提交：
  - 用户修改控件内容后，点击“确定”提交。
  - 已勾选字段以弹窗中的新值统一覆盖所有已勾选对象的对应字段；未勾选字段保持原值。
  - 提交前校验必填项、格式、取值范围和依赖关系，校验失败时在对应字段行给出错误提示并阻止提交。
- 结果反馈：
  - 全部成功：提示成功数量，关闭弹窗，刷新列表数据并清除失效的勾选状态。
  - 部分失败：提示“成功 X 个，失败 Y 个”，展示失败对象和原因。
  - 全部失败：明确提示失败，展示失败对象和原因；保留弹窗内容和已填写值以便修正后重试。
- 边界情况：
  - 未勾选任何字段时点击“确定”：禁用确定按钮，或提示“请先勾选要编辑的字段”。
  - 未勾选对象时入口不可达：“批量编辑”按钮保持禁用，悬浮提示“请先勾选对象再执行操作”。
  - 提交过程中按钮进入 loading，防止重复提交。
  - 当前勾选对象的字段集不一致时，仅展示所有勾选对象均支持的公共字段。

## 复用规则
- 入口按钮位置、未勾选禁用、勾选数量限制等规则复用 `../03-patterns/02-solution-patterns/table-patterns.md` 表格头部操作区规范。
- 批量操作结果反馈（成功、部分失败、全部失败）复用 `../03-patterns/02-solution-patterns/table-patterns.md` 批量操作反馈规则。
- 字段控件映射复用 `../03-patterns/02-solution-patterns/form-patterns.md` 常见字段组件映射速查。
- 弹窗关闭等交互复用 `../03-patterns/04-interaction-patterns.md` 交互反馈与风险控制规则。
