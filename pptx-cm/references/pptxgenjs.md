# PptxGenJS 参考

本文件只负责 `pptx-cm` 中最常用、最容易踩坑的 `PptxGenJS` 用法。代码示例保持原样，解释统一使用中文。

## 初始化与基本结构

```js
const pptxgen = require("pptxgenjs");

let pres = new pptxgen();
pres.layout = 'LAYOUT_16x9';  // or 'LAYOUT_16x10', 'LAYOUT_4x3', 'LAYOUT_WIDE'
pres.author = 'Your Name';
pres.title = 'Presentation Title';

let slide = pres.addSlide();
slide.addText("Hello World!", { x: 0.5, y: 0.5, fontSize: 36, color: "363636" });

pres.writeFile({ fileName: "Presentation.pptx" });
```

## 页面尺寸

坐标单位是英寸。开始写页面前，先固定版式尺寸，避免中途换算漂移。

---

## 文本与格式

常见文本写法：

```js
slide.addText("Simple Text", {
  x: 1, y: 1, w: 8, h: 2, fontSize: 24, fontFace: "Arial",
  color: "363636", bold: true, align: "center", valign: "middle"
});
```

字符间距：

```js
slide.addText("SPACED TEXT", { x: 1, y: 1, w: 8, h: 1, charSpacing: 6 });
```

注意：

- 标题区对齐要求高时，`margin: 0` 很有用
- 中文演示文稿中应显式设置 `Microsoft YaHei`，并在导出后复核是否真的生效

---

## 列表与要点项

优先使用数组形式清晰控制要点项。

不要写成这种容易产生双重要点符号的形式：

```js
slide.addText("- First item", { ... });  // Creates double bullets
```

---

## 表格兼容性说明

### 安全默认值

加表格时建议先从最保守配置开始，再逐步加样式：

- 边框简单
- 字体简单
- 对齐简单
- 不做复杂全局样式

先在 PowerPoint 里打开结果，再逐渐增加美化。

### 已知高风险写法

避免在表格级别使用：

```js
slide.addTable(rows, {
  x: 0.6, y: 1.0, w: 12.0,
  valign: "mid"
});
```

在 `pptx-cm` 本地测试里，表格级 `valign: "mid"` 可能出现：

- zip 可以正常解开
- XML 看起来也正常
- 但 PowerPoint 无法正常打开

### 高风险表格样式组合

如果把以下项大量打包到表格级样式里，风险会升高：

- `fontFace: "Microsoft YaHei"`
- `fontSize`
- `color`

### 优先恢复顺序

如果怀疑生成的演示文稿因表格页打不开：

1. 先移除表格级 `valign`
2. 再移除 bundled text-style 选项
3. 再回退到更简单的边框和填充
4. 重新生成并在 PowerPoint 中复验

### 实操规则

表格能保守就保守；先求兼容，再求精致。

---

## 形状

矩形、线条、圆角矩形等形状都可作为模块化页面的骨架。`pptx-cm` 中更推荐用它们搭出稳定结构，而不是依赖复杂阴影或特效。

阴影可以用，但要克制：

- 轻
- 少
- 不喧宾夺主

---

## 图片

### 图片来源

- 本地路径
- base64
- 远程路径（如环境允许）

### 图片选项

常见参数包括：

- `rotate`
- `rounding`
- `transparency`
- `flipH`
- `flipV`
- `altText`
- `hyperlink`

### 图片尺寸模式

放图时优先守住比例，不要为了填满框而暴力拉伸。

### 计算尺寸

当需要按比例缩放时，先算长宽，再决定居中或贴边。

### 支持格式

实际使用前，优先选择稳定、清晰、兼容的图片格式。

---

## 图标

如果要在 Node 环境里动态渲染图标，可用 `react-icons + react + react-dom + sharp` 路线。

适合：

- 小型能力图标
- 状态标记
- 指标页中的辅助视觉符号

不适合：

- 替代主要图表
- 把整页做成大量装饰图标堆砌

## 页面背景

可以使用：

- 纯色背景
- 半透明背景
- 背景图
- base64 背景图

但在中国移动风格里，背景通常应保持克制，不要为了“高级感”牺牲信息可读性。

---

## 表格

表格适合：

- 精确数字
- 多属性对比
- 投资 / 决策 / 资源类信息

不适合：

- 把原本应做成卡片或流程图的内容硬塞进表格

---

## 图表

基础类型：

- 柱状图
- 折线图
- 饼图

更好看的图表并不只靠默认配置。建议：

- 用更干净的配色
- 弱化多余边框
- 减轻网格线
- 给关键数值更清晰的标签
- 图表边上补一条判断，而不是只放图

默认图表通常偏旧，需要主动做“现代化清理”。

---

## Slide Master（幻灯片母版）

如果页面样式高度重复，可以考虑定义 `Slide Master（幻灯片母版）`。

适合：

- 统一标题页
- 统一章节页
- 大批量格式一致的内容页

但如果任务强调“贴参考稿微调”，不要过早抽象成过重的母版。

---

## 常见坑

1. 颜色值不要写 `#FF0000` 这种形式，按库要求写纯十六进制字符串。
2. 阴影对象复用不当时，容易出现意外副作用。
3. 中文字体声明了，不代表最终导出就真的保留了。
4. 页面坐标如果前后体系不一致，很容易出现越做越偏的问题。
5. 表格是 `pptx-cm` 里最需要优先怀疑兼容性的区域。

---

## 快速参考

- 先定版式尺寸
- 先搭结构，再填内容
- 先保兼容，再加样式
- 先让 PowerPoint 能打开，再谈精细美化
- 最终一定做渲染 QA
