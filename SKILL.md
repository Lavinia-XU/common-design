---
name: common-design
description: 提供B端产品通用设计规范与设计知识，覆盖主题设计、功能点模式、基础页面规则、组件映射等；供prd-design-code在需求设计阶段按需读取
metadata:
  skill_type: common-design
  scope: b2b-product-design
  capability: product-design-knowledge
  version: "1.0"
  frieren.tags: "通用设计"
---

# 安全产品通用设计库

## 任务目标

- 本 Skill 用于：为 `prd-design-code` 或其他设计类 Skill 提供安全产品通用设计规范知识源。
- 能力包含：主题类设计模式、功能点设计模式、基础页面结构、表格与表单规则、交互与状态规范、产品文案术语、idux组件映射和产品导航参考。
- 触发条件：当设计方案需要补齐页面结构、导航归属、页面层级、功能点交互、状态边界、文案术语或组件选型时读取本 Skill。
- 边界说明：本 Skill 不独立执行 PRD 拆解，不负责生成页面，不负责生成HTML说明书，不负责输出Coding计划，也不直接执行Coding。

## 使用方式

### 作为设计规范知识源使用

- 先由主任务 Skill 判断当前设计问题属于主题模式、功能点模式、基础设计、组件映射还是产品导航。
- 只读取与当前设计问题直接相关的参考文件，避免一次性加载全部资料。
- 若业务设计 Skill 已明确给出产品专属规则，以业务设计 Skill 为准；本 Skill 仅用于补充业务设计 Skill 未覆盖或未说清的通用规则。
- 若用户需求、业务设计 Skill 和本 Skill 出现冲突，按以下优先级处理：用户明确要求 > 业务设计 Skill > 本 Skill 通用规范 > AI基于上下文的合理补齐。

### 推荐读取顺序

1. 需求命中策略、任务等完整业务主题时，先读取 `references/01-theme-patterns/` 下对应主题文档。
2. 页面包含导入、导出、标签、优先级、执行周期等局部能力时，读取 `references/02-theme-function-patterns/` 下对应功能点文档。
3. 需要判断页面类型、导航层级、表格、表单、交互、状态或术语时，读取 `references/03-design-template/` 下对应基础设计文档。
4. 需要把设计语义落到 idux 组件时，读取 `references/04-components/idux-component-map.md`。
5. 缺少业务设计 Skill 或业务设计 Skill 未覆盖产品菜单结构时，读取 `references/05-product-navigation/navigation-index.md` 并按产品线进入对应导航文档。

## 使用示例

- 示例1：
  - 场景/输入：`prd-design-code` 已拆出一个策略管理类页面，但需要判断页面结构和常见操作。
  - 读取资源：先读 `references/01-theme-patterns/strategy-management.md`，再按需读取表格、表单和交互规范。
  - 预期用途：补齐策略列表、新增编辑、优先级调整、启用禁用、删除确认等设计依据。

- 示例2：
  - 场景/输入：页面表单中包含标签字段，同时页面还需要标签管理入口。
  - 读取资源：读取 `references/02-theme-function-patterns/tag-management.md`。
  - 预期用途：明确标签配置、标签创建、标签管理入口、标签同步关系和实现检查点。

- 示例3：
  - 场景/输入：需求属于DSP产品，但未提供业务设计 Skill，需要判断页面应放在哪个菜单下。
  - 读取资源：先读 `references/05-product-navigation/navigation-index.md`，再读 `references/05-product-navigation/dsp-navigation.md`。
  - 预期用途：辅助判断导航归属、菜单命名规律、Tab页面关系和是否存在重名冲突。

## 资源索引

- 主题设计库：见 [references/01-theme-patterns/strategy-management.md](references/01-theme-patterns/strategy-management.md)（何时读取：需求涉及策略、规则、白名单、规则组、策略优先级或策略生命周期时）
- 主题设计库：见 [references/01-theme-patterns/task-management.md](references/01-theme-patterns/task-management.md)（何时读取：需求涉及任务管理、升级任务、分发任务、扫描任务、周期执行、定时执行、执行进度或失败重试时）
- 主题设计库：见 [references/01-theme-patterns/live-preview-config.md](references/01-theme-patterns/live-preview-config.md)（何时读取：需求涉及实时预览配置、效果预览、全屏预览、水印配置、重定向页面配置或自定义提示页配置时）
- 功能点设计库：见 [references/02-theme-function-patterns/tag-management.md](references/02-theme-function-patterns/tag-management.md)（何时读取：需求涉及标签配置、标签筛选、标签创建、标签管理或标签与对象关联时）
- 功能点设计库：见 [references/02-theme-function-patterns/import.md](references/02-theme-function-patterns/import.md)（何时读取：需求涉及批量导入、模板下载、文件解析、导入校验或失败明细时）
- 功能点设计库：见 [references/02-theme-function-patterns/export.md](references/02-theme-function-patterns/export.md)（何时读取：需求涉及批量导出、导出范围、导出字段、文件生成或文件下载时）
- 功能点设计库：见 [references/02-theme-function-patterns/execution-cycle.md](references/02-theme-function-patterns/execution-cycle.md)（何时读取：需求涉及执行周期、定时任务周期、每天每周每月联动选择或周期表单配置时）
- 功能点设计库：见 [references/02-theme-function-patterns/priority-configuration.md](references/02-theme-function-patterns/priority-configuration.md)（何时读取：需求涉及优先级、策略位置、规则位置、移动到某条数据之前或之后时）
- 基础设计库：见 [references/03-design-template/01-page-types.md](references/03-design-template/01-page-types.md)（何时读取：判断页面类型、页面骨架、标题栏、内容区结构、底部操作和线框图结构时）
- 基础设计库：见 [references/03-design-template/02-navigation-and-hierarchy.md](references/03-design-template/02-navigation-and-hierarchy.md)（何时读取：设计菜单归属、页面拆解、详情页、Tab、页面层级和内容组织时）
- 基础设计库：见 [references/03-design-template/03-table-patterns.md](references/03-design-template/03-table-patterns.md)（何时读取：设计表格、工具栏、搜索筛选、字段展示、分页排序、批量操作和行内操作时）
- 基础设计库：见 [references/03-design-template/04-form-patterns.md](references/03-design-template/04-form-patterns.md)（何时读取：设计表单、配置项、步骤条、字段校验、下拉选择和未保存提醒时）
- 基础设计库：见 [references/03-design-template/05-interaction-patterns.md](references/03-design-template/05-interaction-patterns.md)（何时读取：设计二次确认、高危操作、反馈方式、弹窗抽屉边界和操作风险控制时）
- 基础设计库：见 [references/03-design-template/06-state-patterns.md](references/03-design-template/06-state-patterns.md)（何时读取：补充空状态、加载态、异常态、无权限、搜索无结果和极端情况时）
- 基础设计库：见 [references/03-design-template/07-copywriting-terminology.md](references/03-design-template/07-copywriting-terminology.md)（何时读取：生成或校准产品界面文案、按钮文案、术语命名、提示语和状态说明时）
- 组件映射表：见 [references/04-components/idux-component-map.md](references/04-components/idux-component-map.md)（何时读取：需要把页面语义映射为idux组件、按钮、弹窗、抽屉、搜索筛选或描述列表组件时）
- 产品导航索引：见 [references/05-product-navigation/navigation-index.md](references/05-product-navigation/navigation-index.md)（何时读取：缺少业务设计Skill、业务设计Skill未覆盖导航结构，或需要辅助判断产品菜单归属时）
- 产品导航：见 [references/05-product-navigation/atrust-navigation.md](references/05-product-navigation/atrust-navigation.md)（何时读取：需求属于aTrust或涉及零信任、安全接入、互联网安全访问、数据保护等aTrust菜单归属时）
- 产品导航：见 [references/05-product-navigation/dr-navigation.md](references/05-product-navigation/dr-navigation.md)（何时读取：需求属于DR下一代端点安全产品，需要判断安全监控、检测响应、病毒查杀、客户端、策略中心等菜单归属时）
- 产品导航：见 [references/05-product-navigation/dsp-navigation.md](references/05-product-navigation/dsp-navigation.md)（何时读取：需求属于DSP数据安全平台，需要判断数据资产、访问可视、风险监测、调查审计或配置管理菜单归属时）
- 产品导航：见 [references/05-product-navigation/sase-navigation.md](references/05-product-navigation/sase-navigation.md)（何时读取：需求属于SASE云安全访问服务，需要判断零信任、数据防泄密、互联网安全访问、终端或对象管理菜单归属时）
- 产品导航：见 [references/05-product-navigation/xdr-navigation.md](references/05-product-navigation/xdr-navigation.md)（何时读取：需求属于XDR，需要判断安全监控、威胁分析、威胁响应、风险管理、资产中心或配置管理菜单归属时）

## 注意事项

- 本 Skill 只提供规范知识，不直接产出页面方案、HTML文件或代码。
- 使用时应按需读取参考文件，优先读取最小相关范围。
- 主题设计提供框架参考，不代表字段、操作和页面层级必须照搬；实际设计仍需结合业务需求和业务设计 Skill 调整。
- 功能点设计用于补齐局部能力，放入页面方案时应简要说明设计要点，并在后续设计或编码指导中标明关联依据。
- 产品导航仅作为菜单归属、命名规律和页面层级判断依据，不替代具体业务设计规范。
