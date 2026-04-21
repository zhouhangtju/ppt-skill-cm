# pptx-cm 技能说明

`pptx-cm` 是一个面向 技术调研汇报风格的 PowerPoint 专用技能，用于创建、编辑、分析和视觉 QA `.pptx` 文件。

它不是普通的 PPT 生成器，而是一套完整的技术调研型 PPT 工作流，覆盖内容提炼、页纲规划、页面模式选择、视觉规范、PPT 生成/编辑、PowerPoint 兼容检查和逐页渲染 QA。

## 适用场景

适合使用 `pptx-cm` 的任务包括：

- 从 Markdown、研究报告、笔记或提纲生成 PPT
- 创建中文技术研究、产业分析、竞品分析、内部汇报类 PPT
- 编辑、改写、扩展或填充已有 `.pptx`
- 读取、总结、分析已有 PPT 内容
- 按高密度蓝白研究风格制作汇报材料
- 对生成或编辑后的 PPT 做逐页视觉 QA

## 技能目录

技能目录位于：

```text
C:\Users\tanzhiyao\.codex\skills\pptx-cm
```

主要文件结构：

```text
pptx-cm/
├── SKILL.md
├── references/
│   ├── design-template.md
│   ├── workflow.md
│   ├── page-patterns.md
│   ├── example-signatures.md
│   ├── pptx-editing.md
│   └── pptxgenjs.md
└── scripts/
    ├── add_slide.py
    ├── clean.py
    ├── thumbnail.py
    ├── __init__.py
    └── office/
        ├── unpack.py
        ├── pack.py
        ├── validate.py
        ├── soffice.py
        ├── helpers/
        ├── validators/
        └── schemas/
```

## 文件组成

当前技能目录约包含：

| 类型 | 数量 | 作用 |
|---|---:|---|
| `.md` | 7 | 技能入口、设计规范、工作流、页面模式、PPT 编辑和 PptxGenJS 指南 |
| `.py` | 16 | Office 文件解包、打包、清理、验证、缩略图和辅助处理脚本 |
| `.xsd` | 39 | Office Open XML schema，用于校验 PPTX/DOCX/XLSX 内部 XML 结构 |

## 核心入口

### `SKILL.md`

`SKILL.md` 是技能的总入口文件，定义了：

- 什么时候使用 `pptx-cm`
- 新建、编辑、读取、QA 的路线选择
- 技术调研型 PPT 的章节导页规则
- 多章节 PPT 的章节导页规则
- 内容抽取和页纲规划要求
- 生成 PPT 的实现路线
- 最终 QA 的强制要求

它规定了几个硬性原则：

- 不能跳过页纲规划
- 多章节报告不能跳过章节导页
- 不能伪造缺失事实
- 不能使用弱布局替代成熟页面模式
- 生成或重大编辑后必须导出图片逐页检查
- `PptxGenJS` 生成稿必须通过 PowerPoint 打开或渲染验证

## references 目录

`references` 目录是技能的方法论和设计规范区，决定 PPT 应该如何组织、如何设计、如何表达。

### `design-template.md`

定义技术调研型 PPT 的默认视觉模板。

主要内容包括：

- 16:9 宽屏画布
- 蓝白主色，红色强调
- 硬边框、弱阴影、高信息密度
- 中文使用 `Microsoft YaHei`
- 英文和数字使用 `Arial`
- 标题、导语、模块标题、正文、页脚的字号规范
- 章节目录页的三种变体
- 技术研究报告的默认章节逻辑

默认章节逻辑通常是：

```text
1. 概述与简介
2. 背景与发展
3. 原理与架构
4. 能力特点分析
5. 竞争力分析
6. 应用与落地
7. 总结与建议
```

### `workflow.md`

定义完整执行流程。

标准步骤包括：

1. 分类任务类型
2. 抽取原始内容
3. 构建 PPT 页纲
4. 填充页面内容
5. 选择页面模式
6. 应用统一设计规则
7. 处理图表和表格
8. 导出图片并逐页 QA
9. 修复问题并重新验证

它强调：如果没有完成逐页图片检查，不能宣称 PPT 已完成。

### `page-patterns.md`

定义可复用页面模式，防止生成普通、空泛、弱结构页面。

主要页面模式包括：

| 页面模式 | 用途 |
|---|---|
| Chapter Directory Page | 每个章节前的目录式章节页 |
| Research Evidence Mosaic | 多证据、多事实、多事件页面 |
| Observation Bar + Case Grid + Judgment Strip | 应用落地、行业案例页面 |
| Comparison Table + Verdict Sidebars | 竞品、模型、厂商对比页面 |
| Architecture Evolution Row + Explanation Blocks | 架构演进、技术路线页面 |
| Metrics Cluster + Support Modules | 概述、关键数字、指标页 |
| Summary Cards + Recommendations Grid | 总结与建议页 |

明确禁止的弱布局包括：

- 大标题加几个空白白框
- 应用页只有宽泛分类，没有案例颗粒度
- 章节页只是普通文字列表
- 页面没有结论、观察或判断装置
- 内容很丰富却做成过度留白的大字页

### `example-signatures.md`

用于分析参考 PPT 的风格特征。

内置两类风格签名：

- DeepSeek 手写研究型风格：证据密集、红色强调、强叙事、页面判断明显
- Manus/Qwen 模块化清爽风格：蓝白模块、对齐严格、结构稳定、几何节奏清晰

如果用户提供参考 PPT 或截图，应优先继承参考稿风格，而不是直接套默认模板。

### `pptx-editing.md`

说明如何编辑已有 PPT。

典型流程：

```bash
python scripts/office/unpack.py input.pptx unpacked
# 修改 unpacked 目录中的 XML
python scripts/clean.py unpacked
python scripts/office/pack.py unpacked output.pptx --original input.pptx
```

它还说明了：

- 如何复制 slide
- 如何清理孤儿文件
- 如何处理 XML 文本和格式
- 如何避免 Unicode bullet、smart quotes、命名空间损坏等问题

### `pptxgenjs.md`

说明从零生成 PPT 时如何使用 `PptxGenJS`。

覆盖内容包括：

- 页面尺寸
- 文本和富文本
- 列表和 bullet
- 形状和线条
- 图片和图标
- 表格和图表
- 母版
- 常见兼容性问题

其中最重要的兼容性提醒是：`PptxGenJS` 的表格全局样式可能导致 PowerPoint 打不开或导出崩溃，因此复杂表格要保守处理，必要时改成手工绘制矩阵。

## scripts 目录

`scripts` 目录提供底层 Office/PPT 操作工具。

### `add_slide.py`

用于给解包后的 PPTX 增加页面。

支持两种方式：

- 复制已有 slide
- 基于 slide layout 创建新 slide

它会同步更新：

- slide XML
- relationship 文件
- `[Content_Types].xml`
- `presentation.xml` 中的 slide id

### `clean.py`

用于清理解包后的 PPTX。

主要功能：

- 删除未引用的 slide
- 删除孤儿 `.rels` 文件
- 删除未引用的媒体、图表、嵌入对象等资源
- 删除 trash 目录
- 更新 `[Content_Types].xml`

适合在编辑模板或删除页面后使用，避免 PPT 内部残留无效文件。

### `thumbnail.py`

用于生成 PPT 缩略图网格，辅助视觉 QA。

主要用途：

- 快速查看全稿页面顺序
- 检查隐藏页
- 检查明显版式错误
- 对比整体视觉风格是否统一

## scripts/office 目录

`office` 目录是通用 Office 文件处理工具区。

### `unpack.py`

用于解包 Office 文件。

支持：

- `.pptx`
- `.docx`
- `.xlsx`

功能包括：

- 解压 Office zip 包
- 格式化 XML
- 转义智能引号
- 对 DOCX 可选执行 run 合并和红线简化

### `pack.py`

用于将解包目录重新打包成 Office 文件。

功能包括：

- XML 压缩
- schema 验证
- 自动修复
- 保持原始文件结构
- 输出 `.pptx/.docx/.xlsx`

### `validate.py`

用于校验 Office 文件或解包目录。

它会调用 `validators` 中的校验器，并使用 `schemas` 中的 XSD 文件检查 XML 合法性。

### `soffice.py`

LibreOffice 调用辅助脚本。

主要用于：

- 调用 `soffice` 做 PDF 或图片导出
- 在受限环境下处理 AF_UNIX socket 问题
- 为视觉 QA 提供 PowerPoint 兼容渲染替代路线

## scripts/office/helpers 目录

辅助处理 Office XML。

### `merge_runs.py`

合并 DOCX 中格式一致的相邻 run，减少 XML 碎片，便于后续编辑。

### `simplify_redlines.py`

简化 Word 修订痕迹，把相邻且同作者的插入/删除元素合并。

## scripts/office/validators 目录

Office XML 校验器。

| 文件 | 作用 |
|---|---|
| `base.py` | 通用 schema validator 基类 |
| `pptx.py` | PPTX XML 校验器 |
| `docx.py` | DOCX XML 校验器 |
| `redlining.py` | Word 修订痕迹校验器 |
| `__init__.py` | 导出 validator 类 |

这些文件配合 `schemas` 目录使用，用于判断解包后的 Office XML 是否符合标准结构。

## scripts/office/schemas 目录

`schemas` 目录保存 Office Open XML 的 XSD 标准文件。

这些文件不是生成脚本，而是校验器使用的标准定义。

主要分组：

| 分组 | 说明 |
|---|---|
| `ecma/fouth-edition` | OPC 包结构相关 schema，如 content types、relationships、core properties |
| `ISO-IEC29500-4_2016` | Office Open XML 标准 schema，覆盖 PPT、Word、Excel、DrawingML、VML、图表等 |
| `mce` | Markup Compatibility and Extensibility schema |
| `microsoft` | Microsoft Word 扩展 schema |

常见关键文件：

| 文件 | 说明 |
|---|---|
| `pml.xsd` | PowerPoint PresentationML 主 schema |
| `dml-main.xsd` | DrawingML 主 schema，覆盖形状、文本、颜色、线条等 |
| `dml-chart.xsd` | 图表 schema |
| `wml.xsd` | WordprocessingML 主 schema |
| `sml.xsd` | SpreadsheetML 主 schema |
| `opc-relationships.xsd` | Office 包关系文件 schema |
| `opc-contentTypes.xsd` | Office 包内容类型 schema |

## 典型使用路线

### 路线一：从 Markdown 或研究稿新建 PPT

适合从零生成新 deck。

流程：

1. 读取 `SKILL.md`
2. 读取 `design-template.md`、`workflow.md`、`page-patterns.md`
3. 解析源文档结构、关键数字、术语、表格和图示点
4. 规划页纲
5. 每章插入目录式章节页
6. 使用 `PptxGenJS` 生成 `.pptx`
7. 同时保留可复用生成脚本
8. 用 PowerPoint 或 LibreOffice 渲染成图片
9. 逐页检查并修复
10. 最终确认页数、占位符和兼容性

### 路线二：编辑已有 PPT

适合改模板、替换内容、扩展页面。

流程：

```bash
python scripts/office/unpack.py input.pptx unpacked
# 编辑 XML
python scripts/clean.py unpacked
python scripts/office/pack.py unpacked output.pptx --original input.pptx
python scripts/office/validate.py output.pptx
```

之后仍需导出图片逐页 QA。

### 路线三：读取或总结 PPT

适合分析已有 deck。

技能推荐：

```bash
python -m markitdown presentation.pptx
python scripts/thumbnail.py presentation.pptx
```

第一步用于提取文本，第二步用于生成视觉总览。

## 默认视觉风格

| 维度 | 默认规则 |
|---|---|
| 画布 | 16:9 widescreen |
| 风格 | 中文技术研究、产业分析、企业汇报 |
| 主色 | 蓝白 |
| 强调色 | 红色用于关键数字、风险、突破、结论 |
| 字体 | 中文 `Microsoft YaHei`，英文/数字 `Arial` |
| 版式 | 高密度、硬边框、弱阴影 |
| 标题 | 中等偏强，不使用超大标题 |
| 正文 | 小到中字号，强调信息密度和层级 |
| 章节页 | 每章前插入目录式章节页 |
| QA | 必须导出图片逐页检查 |

## 非协商规则

使用 `pptx-cm` 时必须遵守：

- 不跳过页纲
- 不跳过章节导页
- 不跳过逐页图片 QA
- 不伪造源材料中不存在的事实
- 不随机混用视觉语言
- 不使用空泛弱布局
- 不把成熟参考风格降级成普通蓝白模板
- 不留下明显溢出、错位、裁切、孤立标点
- 不把丰富内容做成过度留白的大字页
- 不仅凭 zip/XML 正常就宣布 PPT 完成，必须通过 PowerPoint 打开或渲染验证

## 总结

`pptx-cm` 的核心价值是把 技术调研 PPT 方法沉淀为可执行技能。

它解决的不只是“生成 PPT”，而是：

- 如何从复杂材料中提炼汇报结构
- 如何把内容组织成一页一结论
- 如何选择适合技术研究的页面模式
- 如何保持蓝白高密度研究风格
- 如何避免 AI PPT 常见的空泛和模板化
- 如何通过 PowerPoint 渲染 QA 保证最终可交付

