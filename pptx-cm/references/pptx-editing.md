# PPTX 编辑说明

## 基于模板的工作流

当你要把一份现有演示文稿当作模板来改时：

1. 先快速理解模板结构与视觉语法
   ```bash
   python scripts/thumbnail.py template.pptx
   python -m markitdown template.pptx
   ```
2. 再决定是沿用页面、复制页面，还是复制版式后重填内容
3. 尽量在不破坏原有关系链的前提下修改

---

## 脚本

### `unpack.py`

```bash
python scripts/office/unpack.py input.pptx unpacked/
```

作用：

- 解包 PPTX
- 格式化 XML，便于阅读
- 处理智能引号等常见字符问题

### `add_slide.py`

```bash
python scripts/add_slide.py unpacked/ slide2.xml       # 复制现有页面
python scripts/add_slide.py unpacked/ slideLayout2.xml # 从布局版式创建
```

作用：

- 输出需要添加到 `<p:sldIdLst>` 中的 `<p:sldId>`
- 便于在指定位置插入新页

### `clean.py`

```bash
python scripts/clean.py unpacked/
```

作用：

- 删除不在 `<p:sldIdLst>` 中的页面
- 清理未引用媒体
- 清理孤立 rels

### `pack.py`

```bash
python scripts/office/pack.py unpacked/ output.pptx --original input.pptx
```

作用：

- 校验
- 尝试修复
- 压缩 XML
- 重新编码智能引号等字符

### `thumbnail.py`

```bash
python scripts/thumbnail.py input.pptx [output_prefix] [--cols N]
```

作用：

- 生成页面缩略图概览
- 快速判断模板风格、页面密度和章节结构

---

## 页面操作

页面顺序存放在 `ppt/presentation.xml` 的 `<p:sldIdLst>` 中。

修改页面顺序、删除页面、插入复制页时：

- 先看清 `<p:sldIdLst>`
- 再核对 `ppt/slides/slideX.xml`
- 最后检查关联的 `.rels`

---

## 编辑内容

逐页处理时建议：

1. 先识别标题区、正文区、结论区、图表区
2. 先替换标题和关键结论
3. 再处理正文块、图表块和表格块
4. 最后回看对齐、留白和层级

### 格式规则

- 尽量复用原有文本框，不轻易重建
- 原模板里已有成熟样式时，优先保留 run 级格式（text run 级格式）
- 需要新增块时，尽量复制现有相近块后再改
- 不要随意破坏占位符、关系链和命名结构

---

## 常见坑

### 模板适配

当源内容比模板中的项目更少时：

- 不要留下一串空壳模块
- 可合并、删除或拉伸，但要保持整体节奏

当替换文本长度差异很大时：

- 先收缩内容，再考虑扩容
- 不要第一反应就把字号放很小

### 多项目内容

如果一页中有多组重复结构：

- 先找清楚哪几个 shape / group 属于同一模块
- 批量复制前先确认内部 id / rel 不会冲突
- 复制后及时清理无用引用

### 智能引号

- 从 Word、网页、PDF 粘过来的内容可能带智能引号
- 打包前尽量让文本字符正规化
- 如果 XML 能打开但 PowerPoint 打不开，检查是否存在异常字符

### 其他

- 不要只看 XML 正常就认为文件可用
- 最终仍以 PowerPoint 或兼容渲染结果为准
- 任何结构性修改后都应重新打包并检查
