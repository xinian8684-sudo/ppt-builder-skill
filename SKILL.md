---
name: ppt-builder
description: Use when creating an editable PPTX or slide deck from scripts, outlines, documents, training material, product notes, speeches, or rough presentation ideas; especially when the task needs slide planning, visual structure, pptxgenjs generation, image-ratio handling, or rendered-slide QA.
---

# PPT Builder — 从材料到可编辑 PPTX 的完整交付工作流

## 触发时机

不只是"用户说要做PPT"才触发，以下也该触发：
- 用户说"帮我整理个培训材料"、"新品发布需要演示"
- 用户说"把这个文档做成slides"
- 用户说"准备一个分享用的deck"
- 用户分享了一段长文案/大纲，问"这个怎么呈现"

如果用户只是要"修改一个已有的PPTX文件中的文字"，那是简单编辑任务，不需要走完整流程。

---

## 为什么需要6步确认

PPT是视觉产物，文字到布局之间有巨大的设计决策空间。如果跳过确认直接生成：
- 大纲不对 → 重做整份PPT
- 布局不满意 → 改代码等于重写
- 设计风格跑偏 → 用户拿到手才发现不是想要的

每一步用户确认，代价是一次对话来回；不确认的代价是全部返工。这是从多个项目踩坑后得出的结论。

---

## 完整流程

### Step 1: 提取大纲 → 用户确认

从用户提供的材料（演讲稿、文案、Word文档等）中提取结构化大纲。

如果是 .docx 文件，先用 `python -m markitdown` 提取文本。

**输出格式**（简洁，一行一页）:
```
P01: 封面 — XXX
P02: XXX — 要点1、要点2...
P03: XXX — ...
...
```

**做什么**：提炼N页大纲，每页包含标题 + 3-5个核心要点。呈现给用户，让用户调整页数、顺序、内容重点。

**如果用户给的内容不够填满所需页数**，主动提出建议（拆分某页、增加过渡页、补充数据页），但让用户决策。

### Step 2: 分镜脚本 → 用户确认

这是最关键的一步。每页设计具体布局方案。

**输出格式**:
```
P01: 封面
  - 布局: Logo居中 + 主标题 + 副标题 + 底部信息栏
  - 素材: logo.png, 背景图(如有)
  - 文案: 从演讲稿提取的具体文字

P02: XXX
  - 布局: 左文字右数据卡片 / 2×2宫格 / 左右对比 / 三列图标 ...
  - 素材: xxx.png, xxx.png
  - 文案: ...
```

**为什么这一步最重要**：代码只是翻译器，布局方案才是核心。AI可以生成代码，但布局设计需要人类确认——你知道你的听众喜欢什么风格，AI不知道。

**关键点**:
- 明确每页的布局模式（相邻页不能雷同，至少间隔一页再重复）
- 列出每页需要的素材文件，缺的文件在这一步就指出来
- 布局描述要具体到"左X右Y"、"上X下Y"、"N×N宫格"
- 如果用户没有素材，问是否需要生成、寻找、重绘，或改用纯文字/图形排版。

### Step 3: 设计系统 → 用户确认

从品牌资产或项目需求中提取设计语言。

**输出内容**:
- 色板（主色、辅色、背景、卡片、边框）
- 字体配对（标题+正文，中文推荐 Microsoft YaHei 系列）
- 整体风格方向（1-2句话描述）
- 如果有参考图/效果图，一并展示

**为什么设计系统要单列一步**：20页PPT会定义几十个颜色值，如果写到第10页才发现主色不对，前面全要改。先锁死常量，后面只引用变量名。

**内置设计系统参考（JLPPT/浅色简洁风）**:
```javascript
const RED = "B71C1C";       // 主色-中国红
const RED_DARK = "7F0000";  // 深红
const DARK = "1A1A1A";      // 标题色
const BODY = "444444";      // 正文色
const MUTED = "666666";     // 辅助文字
const CARD_BG = "EEEEEE";   // 卡片背景
const BORDER = "DDDDDD";    // 边框线
const WHITE = "FFFFFF";
const GOLD = "B8860B";      // 金色点缀

const FONT_H = "Microsoft YaHei";
const FONT_B = "Microsoft YaHei Light";
```

**暗黑科技风参考**:
```javascript
const BG = "0D0D0D";        // 深黑背景
const GOLD = "D4A83C";      // 金色主色
const WHITE = "FFFFFF";     // 正文白
const MUTED = "AAAAAA";     // 辅助灰
const CARD_BG = "1A1A1A";   // 卡片深灰
const ACCENT = "2A2A2A";    // 装饰面板
```

### Step 4: 生成PPTX（pptxgenjs）

**为什么用 pptxgenjs 而不是 python-pptx 或 PowerPoint COM**:
- python-pptx: API繁琐，坐标计算心智负担重，适合简单编辑不适合从零构建
- PowerPoint COM: 慢、不稳定、依赖Windows桌面环境
- pptxgenjs: 代码生成，精确控制坐标，适合程序化批量构建

**代码结构模板**:
```javascript
const pptxgen = require("pptxgenjs");
const path = require("path");
const fs = require("fs");

// 设计常量（从Step 3搬过来）
const BG = "0D0D0D", GOLD = "D4A83C", WHITE = "FFFFFF", ...;
const FONT_H = "Microsoft YaHei", FONT_B = "Microsoft YaHei Light";

// 素材目录
const MATERIAL = path.join(__dirname, "material");

// 自动使用resized_版本（节省PPTX体积）
function getImg(name) {
  const resized = path.join(MATERIAL, "resized_" + name);
  if (fs.existsSync(resized)) return resized;
  return path.join(MATERIAL, name);
}

const pres = new pptxgen();
pres.layout = "LAYOUT_WIDE";  // 16:9, 13.33×7.5英寸
pres.author = "...";
pres.title = "...";

// Helper函数 — 每页都会用到的公共元素
function addBg(s, color) { s.background = { color }; }
function addBranding(s, pageNum) { /* Logo + 页码 + 底部分隔线 */ }
function addContentSlide(pageNum, title) { /* 统一正文页框架 */ }

// 逐页构建（用IIFE隔离变量作用域）
// P01: 封面
(function() { ... })();
// P02: ...
// ...

pres.writeFile({ fileName: "output.pptx" });
```

**运行**:
```bash
npm install pptxgenjs
node build_deck_v1.js
```

**关键约束**:
- 颜色只用6位hex（`B71C1C`），不要8位（`FFFFFF88` — pptxgenjs不认）
- 坐标单位英寸，13.33×7.5是16:9宽屏
- 页码不要贴边（x≥12.0, y≤7.05），会裁切
- `paraSpaceAfter` 控制段落间距，默认太紧凑

#### 图片铁律：宁可不用，不可变形

这是被多次踩坑后建立的原则。pptxgenjs 在同时指定 `w` 和 `h` 时会强制拉伸填充，不会自动保持比例；只给一个维度也不可靠。

**流程**:
1. 构建前，用 Python/Pillow 一次性查出所有素材尺寸和宽高比
2. 在代码注释中标注每张图的原始尺寸
3. 显式传入 `w` 和 `h`，保证 `w/h === 源图宽/源图高`
4. **排版去适应图片比例，不是图片去适应排版**
5. **如果某个位置放不下正确比例的图，宁可去掉，不可变形**

```javascript
// 错误：强制指定 w 和 h，不查源图比例 → 图片压扁或拉长
s.addImage({ path: "img.png", x: 0, y: 0, w: 5.0, h: 3.0 });

// 正确：先查源图尺寸（Pillow: Image.open），显式计算
// img.png 原始尺寸 1000×600, AR=1.667
s.addImage({ path: "img.png", x: 0, y: 0, w: 5.0, h: 3.0 }); // 5.0/3.0 = 1.667 ✓
```

### Step 5: 视觉QA（必须两轮）

**为什么不跳过QA**：代码生成的PPT第一次渲染几乎必然有问题——坐标偏移、文字溢出、元素重叠。这些问题肉眼才看得出来。

**5a. 导出幻灯片为图片**

用 PowerPoint COM（Windows）或 LibreOffice（跨平台）:
```python
# Windows: PowerPoint COM
import win32com.client
ppt = win32com.client.Dispatch('PowerPoint.Application')
pres = ppt.Presentations.Open(pptx_path, WithWindow=False)
for i in range(1, pres.Slides.Count + 1):
    pres.Slides(i).Export(f'slide_{i:02d}.png', 'PNG', 1920, 1080)
pres.Close()
```

```bash
# 跨平台: LibreOffice + pdftoppm
python scripts/office/soffice.py --headless --convert-to pdf output.pptx
pdftoppm -jpeg -r 150 output.pdf slide
```

**5b. 逐页视觉检查**:

使用当前环境可用的视觉能力、截图检查工具，或人工逐页检查。不要依赖某个固定供应商或机器特定脚本路径。

检查时重点看渲染结果，而不是只看代码。

**检查清单**:
- 元素重叠（文字穿过图形、线条穿过文字）
- 文字溢出或被裁切
- 元素间距过近（<0.3英寸）
- 边距不足（<0.5英寸）
- 低对比度文字（浅灰在浅色背景、深灰在深色背景）
- 页码/标语与内容碰撞
- 残留占位符文本
- **图片比例是否扭曲**：检查图片内的文字、人脸、印章等是否自然。圆形是否还是圆的，文字是否变形

**5c. 内容QA**:
```bash
python -m markitdown output.pptx | grep -iE "xxxx|lorem|ipsum|placeholder"
```
确认无占位符残留。

**5d. 必须做两轮**: 第一轮发现问题 → 修复 → 重新导出 → 第二轮确认修复生效且无新问题。一版过的PPT罕见。

### Step 6: 修复 → 交付

修复QA发现的问题，重新构建，告知用户文件路径和大小。

---

## 常见陷阱

| 陷阱 | 原因 | 解决办法 |
|------|------|----------|
| 8位hex颜色 | pptxgenjs只认6位RGB | 用实色代替透明色 |
| 图片比例变形 | 不查源图尺寸就设w+h | 必须先查源图AR再显式计算 |
| emoji图标位置漂移 | 不同emoji在PowerPoint中基线不同 | 增加y坐标余量（±0.1英寸） |
| PPTX体积过大 | 原始图片300dpi | Pillow resize到1600px高度，`resized_`前缀 |
| 多图片AR不一致对齐差 | 不同来源图片尺寸不一 | 预处理统一resize到相同尺寸 |
| markitdown中文乱码 | 终端编码问题 | 用python-docx交叉验证 |
| 视觉QA工具不可用 | 当前环境没有可用视觉模型或渲染工具 | 改用人工检查截图，或请用户提供渲染截图 |
| 深色背景文字对比度不足 | 深灰文字(MUTED)在深黑背景上 | 深色主题的MUTED要调亮到#AAAAAA以上 |

---

## 项目约定

- 素材放 `material/` 目录
- 大图预处理：`resized_` 前缀，1600px高度
- 构建脚本命名：`build_deck_v{N}.js`
- 输出命名：`{项目名}_v{N}.pptx`
- QA截图放 `slide_v{N}/` 目录
- 不用的临时生成图放到项目的临时素材目录，最终交付前清理或归档
