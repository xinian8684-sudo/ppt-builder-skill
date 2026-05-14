# PPT Builder — Claude Code Skill

从演讲稿/大纲到 PPTX 演示文稿的完整工作流 skill。适用于 [Claude Code](https://claude.ai/code)。

## 能做什么

- 输入演讲稿、培训文案、产品大纲 → 输出可直接使用的 .pptx 文件
- 自动提炼大纲、设计分镜脚本、建立色彩系统、代码生成、视觉 QA
- 支持浅色简洁风、暗黑科技风等多种设计风格
- 内置图片比例保护机制，不会出现拉伸变形

## 安装

**方式一：直接复制**
```bash
# Claude Code skills 目录
cp SKILL.md ~/.claude/skills/ppt-builder.md
```

**方式二：通过 npx skills**
```bash
npx skills add <repo-url> --skill ppt-builder ~/.claude/skills/
```

## 使用

在 Claude Code 对话中直接说：

- "帮我做一个新品发布会 PPT，基于这个演讲稿"
- "把这个培训文档做成 15 页的 PPT"
- "生成一个暗黑科技风的演示文稿"

Skill 会引导你走完 6 步：大纲 → 分镜 → 设计 → 生成 → QA → 交付。

## 依赖

| 工具 | 用途 | 安装 |
|------|------|------|
| pptxgenjs | PPTX 生成 | `npm install -g pptxgenjs` |
| Python 3 + Pillow | 图片预处理 | `pip install Pillow` |
| markitdown | 文本提取 | `pip install "markitdown[pptx]"` |
| Gemini API | 视觉 QA（可选） | `pip install google-genai` |

## 项目结构

```
your-ppt-project/
├── material/          # 素材图片（logo、证书、产品图等）
│   ├── logo.png
│   └── resized_*.png  # 预处理后的图片（1600px高度）
├── build_deck_v1.js   # 构建脚本
├── slide_v1/          # QA 截图
└── output_v1.pptx     # 最终输出
```

## 许可

MIT
