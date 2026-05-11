# 页面范式

本文件定义 `pptx-cm` 的批准页面范式，以及“输入信息类型 -> 表达形式 -> 页面布局”的选择逻辑。

它只负责回答三件事：

- 当前页的主导信息应该被表达成什么
- 这些信息应该路由到哪种 page pattern
- 每种 pattern 应如何避免退化成弱布局

## 一、使用顺序

为单页选范式时，按以下顺序判断：

1. 先确认本页的页面目标
2. 再确认本页的主导输入类型
3. 再确认是否有强参考页需要继承
4. 最后在本文件中选择批准过的页面范式

## 二、常见单页布局模式

- 标题页
- 目录页
- 左文右图
- 上图下文
- 双栏对比
- 对比表格
- 时间线
- 多卡片网格
- 核心指标页
- 图表主导页
- 章节分隔 / 目录页

中国移动成熟 deck 中常见的组合布局包括：

- 导语 + 多卡片证据区
- 图表 / 表格 + 右侧结论框
- 左引用 / 右分析
- 案例矩阵页
- 观察条 + 案例网格 + 判断条
- 价值链 + 商业化路径
- 里程碑时间线 + 状态条
- 风险矩阵 + 行动侧栏

## 三、样例参考：

### 3.1 基础框架

建议先定义统一基线，再进入各 Pattern 代码。

```js
const pptxgen = require("pptxgenjs");

const pptx = new pptxgen();
pptx.layout = "LAYOUT_WIDE";

const THEME = {
  titleBlue: "1E88D8",
  deepBlue: "1565C0",
  lightBlue: "EAF4FB",
  text: "1F2937",
  subText: "6B7280",
  border: "D1D5DB",
  danger: "D32F2F",
  card: "FFFFFF",
  bg: "F7FAFD"
};

const FONT = "Microsoft YaHei";

function addHeader(slide, icon, title, rightTitle = "中国移动", rightSub = "") {
  slide.addText(`${icon} ${title}`, {
    x: 0.72, y: 0.42, w: 8.8, h: 0.55,
    fontFace: FONT, fontSize: 28, bold: true, color: THEME.titleBlue,
    margin: 0
  });
  slide.addText(rightTitle, {
    x: 10.9, y: 0.46, w: 1.7, h: 0.24,
    fontFace: FONT, fontSize: 12, bold: true, color: THEME.deepBlue,
    align: "right", margin: 0
  });
  if (rightSub) {
    slide.addText(rightSub, {
      x: 10.3, y: 0.72, w: 2.3, h: 0.2,
      fontFace: FONT, fontSize: 8.5, color: THEME.subText,
      align: "right", margin: 0
    });
  }
}

function addBlueBanner(slide, text) {
  slide.addShape(pptx.ShapeType.roundRect, {
    x: 0.58, y: 1.25, w: 11.9, h: 0.95, rectRadius: 0.08,
    line: { color: THEME.deepBlue, pt: 0.8 },
    fill: { color: THEME.deepBlue, transparency: 0 }
  });
  slide.addShape(pptx.ShapeType.line, {
    x: 0.62, y: 1.25, w: 0, h: 0.95,
    line: { color: THEME.danger, pt: 1.8 }
  });
  slide.addText(text, {
    x: 0.82, y: 1.48, w: 11.3, h: 0.45,
    fontFace: FONT, fontSize: 18.5, bold: true, color: THEME.card,
    breakLine: false, margin: 0
  });
}

function addCard(slide, x, y, w, h, opts = {}) {
  slide.addShape(pptx.ShapeType.roundRect, {
    x, y, w, h, rectRadius: 0.08,
    line: { color: opts.lineColor || THEME.border, pt: 0.8 },
    fill: { color: opts.fillColor || THEME.card },
    shadow: { type: "outer", color: "D9E2F2", blur: 1, angle: 45, distance: 1, opacity: 0.12 }
  });
}

function addTag(slide, text, x, y, w, primary = false) {
  slide.addShape(pptx.ShapeType.roundRect, {
    x, y, w, h: 0.26, rectRadius: 0.12,
    line: { color: primary ? THEME.deepBlue : THEME.border, pt: 0.4 },
    fill: { color: primary ? THEME.deepBlue : THEME.lightBlue }
  });
  slide.addText(text, {
    x, y: y + 0.03, w, h: 0.16,
    fontFace: FONT, fontSize: 7.5,
    color: primary ? THEME.card : THEME.text, align: "center", margin: 0
  });
}
```

映射原则：

- HTML `div.card` -> `addShape(roundRect)` + `addText`
- HTML `display:flex` -> 手动计算区块坐标
- HTML `grid-template-columns` -> 手动按列宽拆分
- HTML `tag` -> 小圆角矩形 + 居中文本
- HTML `metric-card` -> 浅底矩形 + 大数字 + caption
- HTML `blue-banner` -> 顶部整页导语条
- HTML 多页翻页 -> 多张 slide，而不是单页 JS 切换

### 3.2 样例 1：标准蓝侧边栏目录

适用：

- 默认中国移动风格
- 技术研究与正式汇报
- 需要稳健、制度化观感的场景

必备特征：

- 左侧使用明确的蓝色导航区
- 展示完整章节清单
- 仅高亮当前章节
- 其他章节弱化，但仍清晰可见
- 标题区与导航区层次明确

复刻目标：

- 左侧蓝色侧边栏分成亮蓝上区与深蓝下区
- 中部放半透明 `CONTENTS`，下方放白色“目录”
- 右侧 6 条目录胶囊统一起点、统一宽度、统一节奏
- 当前章节使用蓝色编号与蓝色标题，其他章节统一浅灰
- 不使用额外弧线或装饰，保持正式、克制的目录页语法

```js
const pptxgen = require("pptxgenjs");

const pptx = new pptxgen();
pptx.layout = "LAYOUT_WIDE";

const FONT = "Microsoft YaHei";
const C = {
  sidebarTop: "2B8FE2",
  sidebarBottom: "1E59B7",
  sidebarGlow: "83BDF1",
  activeBlue: "5B9DE3",
  white: "FFFFFF",
  gray: "C8C8C8",
  gray2: "B9BEC6",
  shadow: "D7EAFB",
  bg: "FBFDFF"
};

const slide = pptx.addSlide();
slide.background = { color: C.bg };

slide.addShape(pptx.ShapeType.rect, {
  x: 0, y: 0, w: 4.16, h: 7.5,
  line: { color: C.sidebarBottom, pt: 0 },
  fill: { color: C.sidebarBottom }
});
slide.addShape(pptx.ShapeType.rect, {
  x: 0, y: 0, w: 4.16, h: 3.08,
  line: { color: C.sidebarTop, pt: 0 },
  fill: { color: C.sidebarTop }
});

slide.addText("CONTENTS", {
  x: 0.30, y: 2.86, w: 3.56, h: 0.50,
  fontFace: FONT, fontSize: 38, bold: true,
  color: C.sidebarGlow, transparency: 56,
  align: "center", margin: 0
});
slide.addText("目录", {
  x: 1.50, y: 3.48, w: 1.18, h: 0.46,
  fontFace: FONT, fontSize: 40, bold: true,
  color: C.white, align: "center", margin: 0
});

slide.addShape(pptx.ShapeType.line, {
  x: 4.16, y: 0, w: 0, h: 7.5,
  line: { color: "E6EEF8", pt: 0.8 }
});

const items = [
  { no: "01", title: "DeepSeek进展及成绩", active: true, x: 5.52, y: 0.72, w: 5.15 },
  { no: "02", title: "DeepSeek对大模型变革冲击", active: false, x: 5.52, y: 1.82, w: 5.15 },
  { no: "03", title: "DeepSeek发展历程及团队情况", active: false, x: 5.52, y: 2.89, w: 5.15 },
  { no: "04", title: "DeepSeek的算力情况", active: false, x: 5.52, y: 3.96, w: 5.15 },
  { no: "05", title: "DeepSeek核心技术突破", active: false, x: 5.52, y: 5.03, w: 5.15 },
  { no: "06", title: "移动合作畅想", active: false, x: 5.52, y: 6.09, w: 5.15 }
];

function addCapsule(item) {
  slide.addShape(pptx.ShapeType.roundRect, {
    x: item.x, y: item.y, w: item.w, h: 0.66, rectRadius: 0.20,
    line: { color: "EAF2FB", pt: 0.45 },
    fill: { color: C.white },
    shadow: { type: "outer", color: C.shadow, blur: 4, angle: 45, distance: 2, opacity: 0.16 }
  });

  slide.addText(item.no, {
    x: item.x + 0.50, y: item.y + 0.20, w: 0.68, h: 0.22,
    fontFace: FONT, fontSize: 19, bold: true,
    color: item.active ? C.activeBlue : C.gray2,
    align: "center", valign: "mid", margin: 0
  });

  slide.addText(item.title, {
    x: item.x + 1.48, y: item.y + 0.20, w: item.w - 1.92, h: 0.22,
    fontFace: FONT, fontSize: 16, bold: true,
    color: item.active ? C.activeBlue : C.gray,
    valign: "mid", margin: 0
  });
}

items.forEach(addCapsule);
```

避免退化：

- 不要把目录项做成左右错位或长度不一致，那会更像创意封面而不是正式目录页
- 不要让非当前章节过深，否则会和当前章节抢视觉主导权
- 不要在右侧叠加多余弧线、箭头、图标，标准目录页应该以秩序感为主

### 3.3 样例 2：零空白填充策略

#### 样例 3A：卡片底部标签云

适用：

- 卡片主体内容偏少
- 底部有空白，需要补“轻量信息”

```js
const slide = pptx.addSlide();
slide.background = { color: THEME.bg };
addHeader(slide, "🏷", "零空白策略：卡片底部标签云");
addBlueBanner(slide, "标签云不是装饰，而是用于填补底部空白、补充分类信息和强化页面扫描效率。");

addCard(slide, 0.75, 2.45, 5.7, 4.35);
slide.addText("能力模块", {
  x: 1.0, y: 2.75, w: 2.0, h: 0.3,
  fontFace: "Microsoft YaHei", fontSize: 18, bold: true, color: THEME.primary, margin: 0
});
slide.addText("主体内容先讲核心能力、指标或观察，底部用标签云吸收剩余空间。", {
  x: 1.0, y: 3.15, w: 4.8, h: 0.7,
  fontFace: "Microsoft YaHei", fontSize: 11, color: THEME.text, margin: 0
});
addTag(slide, "核心指标A", 1.0, 6.1, 0.95, true);
addTag(slide, "核心指标B", 2.05, 6.1, 0.95, false);
addTag(slide, "核心指标C", 3.1, 6.1, 0.95, false);
addTag(slide, "补充信息", 4.15, 6.1, 0.9, false);
```

#### 样例 3B：右侧流程图填充

适用：

- 左侧内容更重
- 右侧出现大片竖向空白

```js
addCard(slide, 0.75, 2.45, 8.0, 4.35);
addCard(slide, 9.0, 2.45, 2.8, 4.35);

slide.addShape(pptx.ShapeType.roundRect, {
  x: 9.45, y: 3.0, w: 1.9, h: 0.55, rectRadius: 0.06,
  line: { color: THEME.primary, pt: 0.6 }, fill: { color: THEME.primary }
});
slide.addText("步骤1", {
  x: 9.45, y: 3.16, w: 1.9, h: 0.16, align: "center",
  fontFace: "Microsoft YaHei", fontSize: 11, bold: true, color: "FFFFFF", margin: 0
});
slide.addText("↓", {
  x: 10.2, y: 3.72, w: 0.4, h: 0.2, align: "center",
  fontFace: "Microsoft YaHei", fontSize: 20, color: THEME.primary, margin: 0
});
slide.addShape(pptx.ShapeType.roundRect, {
  x: 9.45, y: 4.15, w: 1.9, h: 0.55, rectRadius: 0.06,
  line: { color: THEME.accent, pt: 0.6 }, fill: { color: THEME.accent }
});
slide.addText("步骤2", {
  x: 9.45, y: 4.31, w: 1.9, h: 0.16, align: "center",
  fontFace: "Microsoft YaHei", fontSize: 11, bold: true, color: "FFFFFF", margin: 0
});
```

#### 样例 3C：色块堆叠架构

适用：

- 原材料是一段结构描述
- 需要转成层级关系

```js
slide.addShape(pptx.ShapeType.roundRect, {
  x: 1.0, y: 2.8, w: 3.3, h: 0.6, rectRadius: 0.06,
  line: { color: THEME.primary, pt: 0.5 }, fill: { color: THEME.primary }
});
slide.addText("顶层：业务编排中心", {
  x: 1.18, y: 3.0, w: 2.9, h: 0.16,
  fontFace: "Microsoft YaHei", fontSize: 11, bold: true, color: "FFFFFF", margin: 0
});

slide.addShape(pptx.ShapeType.roundRect, {
  x: 1.0, y: 3.55, w: 3.3, h: 0.85, rectRadius: 0.06,
  line: { color: THEME.accent, pt: 0.5 }, fill: { color: THEME.accent }
});
slide.addText("中层：五大能力中心\n故障 / 质量 / 运维 / 编排 / 资源", {
  x: 1.18, y: 3.78, w: 2.9, h: 0.36,
  fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: "FFFFFF", margin: 0
});

slide.addShape(pptx.ShapeType.roundRect, {
  x: 1.0, y: 4.55, w: 3.3, h: 0.85, rectRadius: 0.06,
  line: { color: THEME.textSecondary, pt: 0.5 }, fill: { color: THEME.textSecondary }
});
slide.addText("底层：N 个专业系统\n无线 / 传输 / 核心网", {
  x: 1.18, y: 4.78, w: 2.9, h: 0.36,
  fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: "FFFFFF", margin: 0
});
```

#### 样例 3D：网格填充

适用：

- 列表型能力、场景、模块较多
- 适合密集扫描

```js
const gridX = 7.0;
const gridY = 2.9;
const cellW = 1.15;
const cellH = 0.82;
const gap = 0.12;
["开通卡单自愈", "传输故障处置", "核心网巡检", "指标归因", "资源诊断", "脚本审核"].forEach((label, i) => {
  const col = i % 3;
  const row = Math.floor(i / 3);
  const x = gridX + col * (cellW + gap);
  const y = gridY + row * (cellH + gap);
  slide.addShape(pptx.ShapeType.roundRect, {
    x, y, w: cellW, h: cellH, rectRadius: 0.06,
    line: { color: "D9E6F5", pt: 0.5 }, fill: { color: "F3F8FE" }
  });
  slide.addText(`🔧\n${label}`, {
    x: x + 0.08, y: y + 0.12, w: cellW - 0.16, h: 0.5,
    fontFace: "Microsoft YaHei", fontSize: 9.5, bold: true,
    color: THEME.primary, align: "center", valign: "mid", margin: 0
  });
});
```

### 3.4 样例 3：Pattern A 三实体对比

适用：

- 三家运营商
- 三个产品方案
- 三个候选路线

复刻目标：

- 顶部导语 Banner
- 中部 3 张高密度差异卡
- 底部 1 张共性总结表

```js
const slide = pptx.addSlide();
slide.background = { color: THEME.bg };
addHeader(slide, "⚖", "Pattern A：三实体对比");
addBlueBanner(slide, "三大主体已形成差异化能力分工，共同推动平台能力向更高成熟度演进。");

const cardY = 2.45, cardH = 3.45, cardW = 3.78, gap = 0.18;
const xList = [0.75, 0.75 + cardW + gap, 0.75 + (cardW + gap) * 2];

xList.forEach((x, idx) => addCard(slide, x, cardY, cardW, cardH));

[
  { x: xList[0], color: THEME.primary, icon: "📱", title: "中国移动", sub: "2+5+N 架构", a: "L3.5", b: "150+", tag1: "40亿+调用", tag2: "自动驾驶" },
  { x: xList[1], color: THEME.accent, icon: "☁️", title: "中国联通", sub: "全国集约化", a: "500万+", b: "亿+", tag1: "MTTR↓11.5%", tag2: "损失↓5%" },
  { x: xList[2], color: THEME.secondary, icon: "🛡️", title: "中国电信", sub: "安全与智能运维", a: "80+", b: "92%", tag1: "云网协同", tag2: "安全管控" }
].forEach(item => {
  slide.addShape(pptx.ShapeType.rect, {
    x: item.x, y: cardY, w: cardW, h: 0.72,
    line: { color: item.color, pt: 0 }, fill: { color: item.color }
  });
  slide.addText(`${item.icon} ${item.title}\n${item.sub}`, {
    x: item.x + 0.18, y: cardY + 0.14, w: 2.2, h: 0.35,
    fontFace: "Microsoft YaHei", fontSize: 13, bold: true, color: "FFFFFF", margin: 0
  });
  addCard(slide, item.x + 0.18, cardY + 0.95, 1.55, 0.8, { fillColor: "F4F7FC" });
  addCard(slide, item.x + 1.88, cardY + 0.95, 1.55, 0.8, { fillColor: "F4F7FC" });
  slide.addText(item.a, {
    x: item.x + 0.34, y: cardY + 1.18, w: 1.2, h: 0.22,
    fontFace: "Microsoft YaHei", fontSize: 20, bold: true, color: item.color, align: "center", margin: 0
  });
  slide.addText(item.b, {
    x: item.x + 2.04, y: cardY + 1.18, w: 1.2, h: 0.22,
    fontFace: "Microsoft YaHei", fontSize: 20, bold: true, color: item.color, align: "center", margin: 0
  });
  slide.addText("指标A", {
    x: item.x + 0.34, y: cardY + 1.47, w: 1.2, h: 0.12,
    fontFace: "Microsoft YaHei", fontSize: 8, color: THEME.textSecondary, align: "center", margin: 0
  });
  slide.addText("指标B", {
    x: item.x + 2.04, y: cardY + 1.47, w: 1.2, h: 0.12,
    fontFace: "Microsoft YaHei", fontSize: 8, color: THEME.textSecondary, align: "center", margin: 0
  });
  addTag(slide, item.tag1, item.x + 0.18, cardY + 2.86, 1.1, true);
  addTag(slide, item.tag2, item.x + 1.38, cardY + 2.86, 0.95, false);
});

slide.addTable([
  [{ text: "维度", options: { bold: true, color: "1E3A8A", fill: "EAF4FB" } },
   { text: "移动", options: { bold: true, color: "1E3A8A", fill: "EAF4FB" } },
   { text: "联通", options: { bold: true, color: "1E3A8A", fill: "EAF4FB" } },
   { text: "电信", options: { bold: true, color: "1E3A8A", fill: "EAF4FB" } }],
  ["核心优势", "规模与平台化", "全国集约治理", "安全闭环能力"],
  ["本页目的", "用三卡讲差异，用表格讲共性", "", ""]
], {
  x: 0.75, y: 6.1, w: 11.55,
  border: { type: "solid", pt: 0.6, color: "D1D5DB" },
  fontFace: "Microsoft YaHei", fontSize: 9.5, color: "1F2937",
  margin: 0.06
});
```

避免退化：

- 不要把三列都做成大白盒 + 少量文字
- 不要只有差异卡，没有共性收束
- 同一组卡片的标题、数字、说明字号必须一致

### 3.5 样例 4：Pattern B 单实体深入

适用：

- 单个方案
- 单个产品
- 一套 AI 助手或平台能力

复刻目标：

- 左侧做视觉锚点
- 右上做场景网格
- 右下做架构链路

```js
const slide = pptx.addSlide();
slide.background = { color: THEME.bg };
addHeader(slide, "🎯", "Pattern B：单实体深入");
addBlueBanner(slide, "基于大模型构建 AI 助手生态，实现故障处置效率 30% 提升。");

slide.addShape(pptx.ShapeType.roundRect, {
  x: 0.78, y: 2.45, w: 2.5, h: 4.3, rectRadius: 0.12,
  line: { color: THEME.primary, pt: 0.6 }, fill: { color: THEME.primary }
});
slide.addText("12", {
  x: 1.1, y: 3.0, w: 1.9, h: 0.7,
  fontFace: "Microsoft YaHei", fontSize: 44, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});
slide.addText("AI 助手场景", {
  x: 1.0, y: 3.85, w: 2.0, h: 0.25,
  fontFace: "Microsoft YaHei", fontSize: 15, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});
slide.addText("30%\n效率提升", {
  x: 1.0, y: 4.8, w: 2.0, h: 0.6,
  fontFace: "Microsoft YaHei", fontSize: 18, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});
slide.addText("星辰大模型 2.0", {
  x: 1.0, y: 5.85, w: 2.0, h: 0.2,
  fontFace: "Microsoft YaHei", fontSize: 11, color: "DDEAFE",
  align: "center", margin: 0
});

addCard(slide, 3.55, 2.45, 8.75, 2.1);
slide.addText("12 大 AI 场景", {
  x: 3.82, y: 2.72, w: 2.0, h: 0.22,
  fontFace: "Microsoft YaHei", fontSize: 16, bold: true, color: THEME.primary, margin: 0
});
["开通卡单自愈", "传输故障处置", "核心网巡检", "指标归因", "脚本审核", "资源诊断", "工单推荐", "知识问答"].forEach((label, i) => {
  const col = i % 4;
  const row = Math.floor(i / 4);
  const x = 3.82 + col * 2.05;
  const y = 3.15 + row * 0.72;
  slide.addShape(pptx.ShapeType.roundRect, {
    x, y, w: 1.75, h: 0.56, rectRadius: 0.05,
    line: { color: "DCE8F6", pt: 0.5 }, fill: { color: "F4F8FE" }
  });
  slide.addText(`🔧 ${label}`, {
    x: x + 0.08, y: y + 0.16, w: 1.58, h: 0.14,
    fontFace: "Microsoft YaHei", fontSize: 8.8, bold: true, color: THEME.primary,
    align: "center", margin: 0
  });
});

addCard(slide, 3.55, 4.75, 8.75, 2.0);
[
  { x: 3.95, color: THEME.primary, title: "网络大模型", sub: "模型层" },
  { x: 6.55, color: THEME.accent, title: "RAG 知识检索", sub: "增强层" },
  { x: 9.15, color: THEME.secondary, title: "12 个 AI 助手", sub: "应用层" }
].forEach((item, idx) => {
  slide.addShape(pptx.ShapeType.roundRect, {
    x: item.x, y: 5.35, w: 1.95, h: 0.72, rectRadius: 0.06,
    line: { color: item.color, pt: 0.6 }, fill: { color: item.color }
  });
  slide.addText(`${item.sub}\n${item.title}`, {
    x: item.x + 0.08, y: 5.53, w: 1.8, h: 0.28,
    fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: "FFFFFF",
    align: "center", margin: 0
  });
  if (idx < 2) {
    slide.addText("→", {
      x: item.x + 2.08, y: 5.52, w: 0.25, h: 0.2,
      fontFace: "Microsoft YaHei", fontSize: 18, color: THEME.primary, margin: 0
    });
  }
});
```

### 3.6 样例 5：Pattern C 漏斗型 / 沙漏型全链路架构

适用：

- 供应链一体化
- 产业互联到消费互联
- 输入-中台-输出型全链路叙事

复刻目标：

- 顶部两端生态入口
- 中间多能力节点
- 底部技术底座

```js
const slide = pptx.addSlide();
slide.background = { color: THEME.bg };
addHeader(slide, "🔺", "Pattern C：漏斗型全链路架构");
addBlueBanner(slide, "全链路一体化架构，从产业互联到消费互联的无缝衔接。");

slide.addShape(pptx.ShapeType.ellipse, {
  x: 0.95, y: 2.55, w: 1.0, h: 1.0,
  line: { color: THEME.primary, pt: 0.7 }, fill: { color: THEME.primary }
});
slide.addText("🏭\n产业互联", {
  x: 1.1, y: 2.82, w: 0.7, h: 0.35, align: "center",
  fontFace: "Microsoft YaHei", fontSize: 9.5, bold: true, color: "FFFFFF", margin: 0
});
slide.addText("⇦", {
  x: 2.08, y: 2.95, w: 0.3, h: 0.2,
  fontFace: "Microsoft YaHei", fontSize: 18, color: THEME.primary, margin: 0
});

["产融合作", "乡村振兴", "产业平台", "双碳治理", "国际业务", "产教融合"].forEach((label, i) => {
  const x = 2.55 + (i % 3) * 1.45;
  const y = 2.65 + Math.floor(i / 3) * 0.48;
  addTag(slide, label, x, y, 1.18, false);
});

slide.addText("⇨", {
  x: 10.25, y: 2.95, w: 0.3, h: 0.2,
  fontFace: "Microsoft YaHei", fontSize: 18, color: THEME.accent, margin: 0
});
slide.addShape(pptx.ShapeType.ellipse, {
  x: 10.55, y: 2.55, w: 1.0, h: 1.0,
  line: { color: THEME.accent, pt: 0.7 }, fill: { color: THEME.accent }
});
slide.addText("🛒\n消费互联", {
  x: 10.7, y: 2.82, w: 0.7, h: 0.35, align: "center",
  fontFace: "Microsoft YaHei", fontSize: 9.5, bold: true, color: "FFFFFF", margin: 0
});

[
  ["📦", "数智采购"],
  ["🔧", "协同开发"],
  ["🏭", "智能制造"],
  ["🔗", "全域链接"],
  ["💎", "价值服务"]
].forEach((item, i) => {
  const x = 1.55 + i * 2.1;
  addCard(slide, x, 4.05, 1.72, 1.2);
  slide.addText(`${item[0]}\n${item[1]}`, {
    x: x + 0.1, y: 4.3, w: 1.52, h: 0.42,
    fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: THEME.primary,
    align: "center", margin: 0
  });
});

slide.addShape(pptx.ShapeType.roundRect, {
  x: 4.35, y: 5.55, w: 4.0, h: 0.45, rectRadius: 0.18,
  line: { color: THEME.primary, pt: 0.6 }, fill: { color: THEME.primary }
});
slide.addText("⚙️ 供应链一体化中台", {
  x: 4.35, y: 5.68, w: 4.0, h: 0.16,
  fontFace: "Microsoft YaHei", fontSize: 11.5, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});

addCard(slide, 1.2, 6.2, 10.9, 0.95, { fillColor: "F1F6FD" });
slide.addText("🔮 数智化技术底座", {
  x: 5.0, y: 6.38, w: 2.4, h: 0.18,
  fontFace: "Microsoft YaHei", fontSize: 11.5, bold: true, color: THEME.primary,
  align: "center", margin: 0
});
["区块链", "人工智能", "大数据", "混合多云", "业务中台", "数字员工"].forEach((label, i) => {
  addTag(slide, label, 1.65 + i * 1.72, 6.7, 1.32, false);
});
```

### 3.7 样例 6：Pattern D 立体平台分层架构

适用：

- 业务平台 + 数据中台 + 技术底座
- 多侧接入、多端输出的平台型系统

复刻目标：

- 左侧业务通道
- 中间三层平台主体
- 右侧承接端

```js
const slide = pptx.addSlide();
slide.background = { color: THEME.bg };
addHeader(slide, "🏗️", "Pattern D：立体平台分层架构");
addBlueBanner(slide, "技术驱动业务变革，构建业务-数据-技术三层立体架构。");

slide.addShape(pptx.ShapeType.roundRect, {
  x: 0.72, y: 2.5, w: 1.25, h: 1.55, rectRadius: 0.08,
  line: { color: THEME.primary, pt: 0.6 }, fill: { color: THEME.primary }
});
slide.addText("To C 业务\n品牌 DTC\nOnline / Offline", {
  x: 0.82, y: 2.85, w: 1.05, h: 0.7,
  fontFace: "Microsoft YaHei", fontSize: 9.5, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});
slide.addShape(pptx.ShapeType.roundRect, {
  x: 0.72, y: 4.25, w: 1.25, h: 1.55, rectRadius: 0.08,
  line: { color: THEME.accent, pt: 0.6 }, fill: { color: THEME.accent }
});
slide.addText("To B 业务\n渠道协同\n订单履约", {
  x: 0.82, y: 4.63, w: 1.05, h: 0.56,
  fontFace: "Microsoft YaHei", fontSize: 9.5, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});

addCard(slide, 2.2, 2.45, 8.45, 4.45);
slide.addShape(pptx.ShapeType.roundRect, {
  x: 2.48, y: 2.75, w: 7.9, h: 0.72, rectRadius: 0.08,
  line: { color: THEME.primary, pt: 0.5 }, fill: { color: THEME.primary }
});
slide.addText("业务平台层\n商城、会员、营销、订单、服务协同", {
  x: 2.68, y: 2.95, w: 7.5, h: 0.26,
  fontFace: "Microsoft YaHei", fontSize: 11.5, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});
[
  { x: 2.6, title: "中台能力", body: "统一商品、统一会员、统一订单", color: "EAF4FB", font: "1E3A8A" },
  { x: 5.25, title: "数据资产", body: "数据采集、清洗、标签、洞察", color: "EEF6FF", font: "1565C0" },
  { x: 7.9, title: "智能引擎", body: "推荐、预测、自动化决策", color: "FFF1F1", font: "D32F2F" }
].forEach(item => {
  slide.addShape(pptx.ShapeType.roundRect, {
    x: item.x, y: 3.82, w: 2.25, h: 1.55, rectRadius: 0.08,
    line: { color: "D1D5DB", pt: 0.5 }, fill: { color: item.color }
  });
  slide.addText(item.title, {
    x: item.x + 0.18, y: 4.08, w: 1.8, h: 0.16,
    fontFace: "Microsoft YaHei", fontSize: 12, bold: true, color: item.font, margin: 0
  });
  slide.addText(item.body, {
    x: item.x + 0.18, y: 4.42, w: 1.8, h: 0.42,
    fontFace: "Microsoft YaHei", fontSize: 9.5, color: THEME.textSecondary, margin: 0
  });
});
slide.addShape(pptx.ShapeType.roundRect, {
  x: 2.48, y: 5.82, w: 7.9, h: 0.62, rectRadius: 0.08,
  line: { color: "D1E4F7", pt: 0.5 }, fill: { color: "F1F6FD" }
});
slide.addText("技术底座：云原生 / API 网关 / 安全治理 / DevOps / 可观测性", {
  x: 2.68, y: 6.02, w: 7.5, h: 0.16,
  fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: THEME.primary,
  align: "center", margin: 0
});

["📱 App", "🏬 门店", "🤝 伙伴"].forEach((label, i) => {
  addCard(slide, 10.95, 2.7 + i * 1.35, 1.3, 1.0);
  slide.addText(label, {
    x: 11.05, y: 3.05 + i * 1.35, w: 1.1, h: 0.18,
    fontFace: "Microsoft YaHei", fontSize: 10, bold: true, color: THEME.primary,
    align: "center", margin: 0
  });
});
```

### 3.8 样例 7：Pattern E 三层横向运营架构

适用：

- 集团 / 区域 / 项目
- 总部 / 分部 / 一线

复刻目标：

- 三层横向职责条带
- 每层展开自身职责网格
- 底部标准体系收束

```js
const slide = pptx.addSlide();
slide.background = { color: THEME.bg };
addHeader(slide, "🏢", "Pattern E：三层横向运营架构");
addBlueBanner(slide, "将集团、区域、项目三层职责拆开表达，核心不是组织图，而是层级职责与联动机制。");

[
  { y: 2.5, color: THEME.primary, label: "集团层", items: ["制度标准", "经营看板", "目标管控", "风险预警"] },
  { y: 3.95, color: THEME.accent, label: "区域层", items: ["片区统筹", "资源调配", "进度分析", "协同联动"] },
  { y: 5.4, color: THEME.secondary, label: "项目层", items: ["任务拆解", "现场执行", "质量检查", "安全巡检", "过程留痕"] }
].forEach(row => {
  slide.addShape(pptx.ShapeType.roundRect, {
    x: 0.72, y: row.y, w: 1.15, h: 1.0, rectRadius: 0.08,
    line: { color: row.color, pt: 0.6 }, fill: { color: row.color }
  });
  slide.addText(row.label, {
    x: 0.82, y: row.y + 0.38, w: 0.95, h: 0.16,
    fontFace: "Microsoft YaHei", fontSize: 11.5, bold: true, color: "FFFFFF",
    align: "center", margin: 0
  });
  const count = row.items.length;
  const cellW = count === 5 ? 1.9 : 2.35;
  row.items.forEach((item, i) => {
    addCard(slide, 2.05 + i * (cellW + 0.12), row.y, cellW, 1.0, { fillColor: "F6FAFE" });
    slide.addText(item, {
      x: 2.05 + i * (cellW + 0.12) + 0.08, y: row.y + 0.37, w: cellW - 0.16, h: 0.16,
      fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: THEME.primary,
      align: "center", margin: 0
    });
  });
});

addTag(slide, "管理标准", 2.55, 6.82, 1.35, false);
addTag(slide, "数据标准", 5.4, 6.82, 1.35, false);
addTag(slide, "作业标准", 8.25, 6.82, 1.35, false);
```

### 3.9 样例 8：Pattern F 立体数据平台架构

适用：

- 数据中台
- 数据平台
- 数据湖仓

复刻目标：

- 左数据源
- 中分层处理
- 右应用出口

```js
const slide = pptx.addSlide();
slide.background = { color: THEME.bg };
addHeader(slide, "🗄️", "Pattern F：数据平台分层");
addBlueBanner(slide, "左源右用，中间分层，让采集、存储、处理、应用的路径天然可读。");

slide.addShape(pptx.ShapeType.roundRect, {
  x: 0.72, y: 2.45, w: 0.95, h: 0.62, rectRadius: 0.08,
  line: { color: THEME.primary, pt: 0.6 }, fill: { color: THEME.primary }
});
slide.addText("Data\n数据源", {
  x: 0.8, y: 2.6, w: 0.8, h: 0.22,
  fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});
["SQL", "Web", "Logs", "IoT"].forEach((item, i) => {
  addCard(slide, 0.72, 3.2 + i * 0.78, 0.95, 0.62);
  slide.addText(item, {
    x: 0.84, y: 3.43 + i * 0.78, w: 0.68, h: 0.12,
    fontFace: "Microsoft YaHei", fontSize: 9.5, color: THEME.textSecondary,
    align: "center", margin: 0
  });
});

addCard(slide, 1.9, 2.45, 9.65, 0.88, { fillColor: "F1F6FD" });
slide.addText("📥 数据获取", {
  x: 5.4, y: 2.64, w: 1.4, h: 0.16,
  fontFace: "Microsoft YaHei", fontSize: 11, bold: true, color: THEME.primary,
  align: "center", margin: 0
});
["消息服务", "结构化同步", "非结构化同步", "流式采集"].forEach((label, i) => {
  addTag(slide, label, 5.25 + i * 0.95 - (i===0?0.4:0), 2.96, 0.9, false);
});

slide.addShape(pptx.ShapeType.roundRect, {
  x: 1.9, y: 3.45, w: 9.65, h: 2.05, rectRadius: 0.08,
  line: { color: "BFD0EB", pt: 0.8 }, fill: { color: "EAF2FC" }
});
slide.addText("🗃️ 数据存储", {
  x: 5.3, y: 3.66, w: 1.6, h: 0.16,
  fontFace: "Microsoft YaHei", fontSize: 12, bold: true, color: THEME.primary,
  align: "center", margin: 0
});
[
  { x: 2.15, title: "原始数据", body: "Raw\nStaging\nResult", color: THEME.primary },
  { x: 4.65, title: "数据集市", body: "仓库\n分析模型\n主题域", color: THEME.accent },
  { x: 7.15, title: "知识融合", body: "关联图谱\n知识图谱", color: THEME.secondary }
].forEach(item => {
  addCard(slide, item.x, 4.05, 2.25, 1.2);
  slide.addText(item.title, {
    x: item.x + 0.12, y: 4.22, w: 1.6, h: 0.14,
    fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: item.color, margin: 0
  });
  slide.addText(item.body, {
    x: item.x + 0.12, y: 4.52, w: 1.6, h: 0.46,
    fontFace: "Microsoft YaHei", fontSize: 9.2, color: THEME.textSecondary, margin: 0
  });
});
addCard(slide, 9.65, 4.05, 1.65, 0.55);
slide.addText("元模型\n元数据 / 本体", {
  x: 9.8, y: 4.18, w: 1.35, h: 0.25,
  fontFace: "Microsoft YaHei", fontSize: 8.2, color: THEME.textSecondary, margin: 0
});
slide.addShape(pptx.ShapeType.roundRect, {
  x: 9.65, y: 4.72, w: 1.65, h: 0.55, rectRadius: 0.06,
  line: { color: THEME.primary, pt: 0.5 }, fill: { color: THEME.primary }
});
slide.addText("Trusted Data", {
  x: 9.75, y: 4.92, w: 1.45, h: 0.12,
  fontFace: "Microsoft YaHei", fontSize: 8.8, bold: true, color: "FFFFFF", margin: 0
});

addCard(slide, 1.9, 5.65, 9.65, 0.88, { fillColor: "F1F6FD" });
slide.addText("🔍 分析处理", {
  x: 5.4, y: 5.84, w: 1.4, h: 0.16,
  fontFace: "Microsoft YaHei", fontSize: 11, bold: true, color: THEME.accent,
  align: "center", margin: 0
});
["探索分析", "函数计算", "工厂任务", "科学工作台", "EMR"].forEach((label, i) => {
  addTag(slide, label, 4.65 + i * 1.0, 6.16, 0.88, false);
});

["可视化", "智能", "搜索", "API"].forEach((label, i) => {
  addCard(slide, 2.1 + i * 2.33, 6.65, 2.1, 0.65, { fillColor: "F5F8FD" });
  slide.addText(label, {
    x: 2.1 + i * 2.33, y: 6.89, w: 2.1, h: 0.12,
    fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: THEME.text,
    align: "center", margin: 0
  });
});

slide.addShape(pptx.ShapeType.roundRect, {
  x: 11.72, y: 2.45, w: 0.95, h: 0.62, rectRadius: 0.08,
  line: { color: THEME.accent, pt: 0.6 }, fill: { color: THEME.accent }
});
slide.addText("Action\n应用", {
  x: 11.8, y: 2.6, w: 0.8, h: 0.22,
  fontFace: "Microsoft YaHei", fontSize: 10.5, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});
["用户", "Web", "移动端", "机器人"].forEach((item, i) => {
  addCard(slide, 11.72, 3.2 + i * 0.78, 0.95, 0.62);
  slide.addText(item, {
    x: 11.84, y: 3.43 + i * 0.78, w: 0.68, h: 0.12,
    fontFace: "Microsoft YaHei", fontSize: 9.5, color: THEME.textSecondary,
    align: "center", margin: 0
  });
});
```

### 3.10 样例 9：反模式修正写法

#### 反模式 A：大段文字直接上页

禁止：

- 用一整段正文解释层级结构

替代：

- 拆成 2-3 个色块
- 每块只承载一个层级

```js
slide.addShape(pptx.ShapeType.roundRect, {
  x: 1.0, y: 2.9, w: 2.0, h: 0.72, rectRadius: 0.06,
  line: { color: THEME.primary, pt: 0.5 }, fill: { color: THEME.primary }
});
slide.addText("双平台\n数据共享 + 通用技术", {
  x: 1.12, y: 3.12, w: 1.76, h: 0.28,
  fontFace: "Microsoft YaHei", fontSize: 10.2, bold: true, color: "FFFFFF",
  align: "center", margin: 0
});
```

#### 反模式 B：纯文本卡片

禁止：

- 卡片里只有标题和一段话

替代：

- 头部图标
- 中部指标
- 底部标签

```js
addCard(slide, 1.0, 2.7, 3.5, 2.7);
slide.addText("☁️ 中国联通", {
  x: 1.18, y: 2.96, w: 1.8, h: 0.2,
  fontFace: "Microsoft YaHei", fontSize: 14, bold: true, color: THEME.primary, margin: 0
});
addCard(slide, 1.18, 3.35, 1.35, 0.82, { fillColor: "F4F8FE" });
addCard(slide, 2.68, 3.35, 1.35, 0.82, { fillColor: "F4F8FE" });
slide.addText("500万+", {
  x: 1.3, y: 3.58, w: 1.1, h: 0.16,
  fontFace: "Microsoft YaHei", fontSize: 18, bold: true, color: THEME.accent, align: "center", margin: 0
});
slide.addText("亿+", {
  x: 2.8, y: 3.58, w: 1.1, h: 0.16,
  fontFace: "Microsoft YaHei", fontSize: 18, bold: true, color: THEME.accent, align: "center", margin: 0
});
addTag(slide, "31省集约", 1.18, 4.82, 0.9, true);
addTag(slide, "AIOps", 2.18, 4.82, 0.75, false);
```

### 3.11 使用这些样例时的落地规则

- 不要逐字复制 HTML 思维中的 `flex` / `grid` 名称；在 `PptxGenJS` 中应改为明确坐标系。
- 不要尝试把 JS 翻页保留到 PPT 中；应直接拆成独立 slide。
- 不要追求浏览器级渐变细节完全一致；先保证模块关系、信息密度、阅读路径一致。
- `ppt-generator` 的“零空白”原则在 `pptx-cm` 中继续生效：
  - 有底部空白就补标签云
  - 有右侧空白就补流程链路
  - 有结构长文就改色块堆叠
  - 有大量分类项就改网格
- 同一组并列卡片必须统一：
  - 标题字号
  - 核心数字字号
  - 说明文字字号
  - 图标尺寸

## 四、禁止事项

| ❌ 禁止               | ✅ 替代方案          |
| -------------------- | ------------------- |
| 大段文字描述         | 色块+图标+数字      |
| 纯文本卡片           | 结构化的metrics卡片 |
| 长句子（除banner外） | 关键词+标签         |
| 均分布局+少量内容    | 非对称+视觉锚点     |
| 信息损失/省略        | 全部数字必须显示    |

## 五、填充检查清单

生成后检查每个卡片：

- [ ] 是否有2+个数字指标？
- [ ] 是否有图标或色块？
- [ ] 底部是否有标签/徽章？
- [ ] 文字是否<20字/项？
- [ ] 架构层次是否清晰（颜色/边框区分）？
- [ ] 流程方向是否明确（箭头/位置关系）？
- [ ] 左右两侧是否有输入/输出标识？
- [ ] 层级标签是否醒目（左侧纵向标签或顶部标识）？
