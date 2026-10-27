# 03-patterns 设计模式库索引

## 概览

- 本目录沉淀需求拆解为页面设计所需的通用设计模式知识，分三层：页面与层级判断（01）、设计解法（02-solution-patterns）、控件级形态（03-widget-patterns），另有交互、状态、文案三个横切规范（04、05、06）。
- 本文件是目录层：只回答「当前设计问题该读哪个文档」，不承载具体设计规则。
- 进入本目录做设计前，先读本文件定位目标文档，再只读命中的文档，禁止全量加载或按文件名猜测。

## 目录结构

| 路径 | 定位 |
|------|------|
| [01-navigation-and-hierarchy.md](01-navigation-and-hierarchy.md) | 页面与层级判断：菜单归属、页面拆解、页面类型选择、详情页与 Tab、内容组织 |
| [02-solution-patterns/](02-solution-patterns/) | 设计解法：典型功能问题的设计方式选型与配套规则，覆盖列表页表格、新增编辑表单、配置方式选型、详情信息展示、概览统计五类问题域，持续扩充 |
| [03-widget-patterns/](03-widget-patterns/) | 控件级形态：具体控件的形态选择与增强项决策 |
| [04-interaction-patterns.md](04-interaction-patterns.md) | 横切规范：二次确认、高危操作、反馈方式、弹窗抽屉边界 |
| [05-state-patterns.md](05-state-patterns.md) | 横切规范：空状态、加载态、异常态、无权限、搜索无结果 |
| [06-copywriting-terminology.md](06-copywriting-terminology.md) | 横切规范：界面文案、按钮文案、术语命名、提示语 |

## 场景路由表

按当前正在做的设计决策查表定位，只读命中的文档：

| 正在做的设计决策 | 读取文档 |
|------------------|----------|
| 页面放哪个菜单、要不要独立页面、页面拆几个、详情页与 Tab 怎么分 | [01-navigation-and-hierarchy.md](01-navigation-and-hierarchy.md) |
| 列表页主体是表格：表格形态（分组、可展开、子母）、工具栏、搜索筛选、字段展示、分页排序、批量与行内操作 | [02-solution-patterns/table-patterns.md](02-solution-patterns/table-patterns.md) |
| 列表/集合条目怎么承载：已配置条目、对接实例、用户源等多实例配置的集合用表格还是多卡片（第三方平台对接、外部存储等场景） | [02-solution-patterns/config-patterns.md](02-solution-patterns/config-patterns.md#7-双容器场景的已配置内容承载) |
| 新增编辑配置：表单布局、配置项聚合、配置项自然语言表达、字段校验、步骤条；设计交付前端实现：表单状态契约、输出结构建议、自检清单 | [02-solution-patterns/form-patterns.md](02-solution-patterns/form-patterns.md) |
| 功能配置选型：配置需要几个容器、新增编辑配置用纯表单、实时预览、手动预览还是可视化编排 | [02-solution-patterns/config-patterns.md](02-solution-patterns/config-patterns.md) |
| 详情页或详情抽屉里每块信息用什么形式展示（描述列表、表格、卡片、时间轴、可视化、原始内容） | [02-solution-patterns/detail-info-patterns.md](02-solution-patterns/detail-info-patterns.md) |
| 概览表格页/概览左树表格页顶部概览统计卡片的样式选择（纯数字、数字与子项、饼图分布、TopN条形、趋势曲线）与组合排序 | [02-solution-patterns/overview-stat-cards.md](02-solution-patterns/overview-stat-cards.md) |
| 下拉选择面板形态（平铺、平铺+搜索、分组+搜索、树+搜索、级联、面板内新增等增强项） | [03-widget-patterns/select-panel.md](03-widget-patterns/select-panel.md) |
| 标签形态（可变更、点状、icon、深色、浅色、浅灰、标签组）与各场景取值 | [03-widget-patterns/tag.md](03-widget-patterns/tag.md) |
| 操作要不要二次确认、反馈用消息还是弹窗抽屉、高危操作风险控制 | [04-interaction-patterns.md](04-interaction-patterns.md) |
| 空状态、加载态、异常态、无权限、搜索无结果等极端情况怎么展示 | [05-state-patterns.md](05-state-patterns.md) |
| 按钮和提示文案怎么写、术语命名是否统一、状态说明措辞 | [06-copywriting-terminology.md](06-copywriting-terminology.md) |

## 典型读取组合

- 列表页设计：01-navigation-and-hierarchy.md 定页面类型 → 判断集合承载形式：多实例配置条目（对接实例、用户源等）按 config-patterns.md 第 7 节定表格或多卡片（判为多卡片用卡片列表页模板），普通对象集合直接用表格 → `../02-template/01-page-types.md` 拿模板线框 → table-patterns.md 补表格规则 → 页面为概览表格页或概览左树表格页时，读 overview-stat-cards.md 定顶部概览卡片样式与组合排序 → 04、05 补交互与状态。
- 新增编辑设计：01-navigation-and-hierarchy.md 判断入口形态 → form-patterns.md 补表单规则 → 涉及下拉、标签等控件读 03-widget-patterns 对应文档 → 06 校准文案 → 要交付前端实现时，再读 form-patterns.md 第 11-13 节（状态契约、输出结构、自检清单）。
- 功能配置设计：01-navigation-and-hierarchy.md 判断入口与容器 → config-patterns.md 定配置逻辑与配置形式 → form-patterns.md 补表单规则 → 双容器场景已配置内容按 config-patterns.md 第 7 节定表格或多卡片，再参照对应文档。
- 详情页设计：01-navigation-and-hierarchy.md 判断要不要详情页与信息落位 → detail-info-patterns.md 定每块信息展示形式 → 05-state-patterns.md 补空态。
- 分工边界：01-navigation-and-hierarchy.md 只做页面判断，不描述展示细节；02-solution-patterns 各文档承载解法结论；03-widget-patterns 各文档承载控件决策；横切规范全局适用。

## 新增约定

- 新增解法文档放入 `02-solution-patterns/`，新增控件文档放入 `03-widget-patterns/`，均不带序号前缀；横切规范直接放本目录根，按现有 01-06 序号延续。
- 新增或重命名文档后，必须同步更新本文件的目录结构与场景路由表，禁止只加文件不登记。
- 本文件只做路由与分工说明；具体规则写进对应文档，同一决策不在此重复描述。
