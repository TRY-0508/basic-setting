---
name: chinese-academic-writing
description: 中文学术论文的排版规范与写作约束。涵盖 xelatex+xeCJK 的中文 PDF 排版、python-docx 的中文 Word 排版、以及中文学术写作的内容规范（术语处理、标点统一、引用格式、概念解释方法等）。当用户要求生成中文学术论文、课程论文、毕设论文，或涉及 LaTeX/Word 中文排版问题时触发。
---

# 中文学术论文写作

## LaTeX 中文排版（xelatex + xeCJK）

1. **不要使用 `\zihao{}` 和 `\CJKfamily{zhhei}`**——这些是 `ctex` 包的命令。若未加载 ctex，使用 `\fontsize{Xpt}{Ypt}\selectfont` 替代 `\zihao`；使用 `\setCJKfamilyfont{hei}{SimHei}` + `\newcommand{\heiti}{\CJKfamily{hei}}` 替代 `\CJKfamily{zhhei}`。**这是最常见的中文 PDF 乱码原因。**
2. **标题格式不要用 `\bfseries`**——`\bfseries` 会泄漏到后续正文中使英文加粗。用 `\heiti` 单独控制黑体即可，SimHei 本身已有足够的视觉重量。
3. **中文引号 ""（U+201C/U+201D）**——这两个字符在 Unicode 中属于通用标点而非 CJK 区段。每次生成含中文引号的 .tex 文件后，务必检查文件中是否混入了 ASCII 双引号 `"` (U+0022)，如有则替换为中文引号。
4. **参考文献 URL 溢出页边距**——在参考文献区域使用 `\sloppy` + `\raggedright`，URL 用 `\url{}` 封装以允许自动断行。
5. **交叉引用的 key 必须是纯字母数字**——`\hyperlink{key}{text}` 和 `\hypertarget{key}{}` 中的 key 不得含空格、`&`、`.` 等特殊字符，否则会导致 PDF 内部链接结构损坏甚至渲染异常。推荐格式：`refAuthorYear`（如 `refShor1995`）。
6. **中文字号对照表**：小二=18pt，三号=16pt，小三=15pt，四号=14pt，小四=12pt，五号=10.5pt，小五=9pt。

## Word（python-docx）中文排版

1. **中文标点的字体问题**——中文引号 ""、破折号 ——等（U+201C/U+201D/U+2014）被 Word 归类为西文字符，默认使用 `w:ascii`/`w:hAnsi` 字体（通常是 Times New Roman）渲染而非 `w:eastAsia` 字体（宋体）。
2. **解决方案**——将段落文本按字符类别拆分为独立 run：中文字符及中文标点（需将 U+201C/U+201D/U+2014 等显式加入中文集合）的 run 的四项字体属性（ascii/hAnsi/eastAsia/cs）全部设为宋体；英文和数字的 run 全部设为 Times New Roman。
3. **交叉引用**——使用 `w:hyperlink`（设 `w:anchor` 属性）指向 `bookmarkStart`/`bookmarkEnd` 定义的锚点。
4. **APA 悬挂缩进**——`left_indent = Cm(0.74)`, `first_line_indent = Cm(-0.74)`。

## 中文学术写作内容规范

1. **标题层级**——论文题目字号必须大于一级标题，后续逐级递减（如：题目小二号 > 一级三号 > 二级四号 > 正文五号）。
2. **术语处理**——首次出现时提供英文：中文（English），后续仅使用中文。避免正文中大量重复英文术语。
3. **标点统一**——正文全部使用中文标点（，。；：——""），仅参考文献部分保留英文标点。
4. **引用文献**——只陈述其工作内容（做了什么），不做评价性判断（如避免"开创性""系统性地"等评价词）。
5. **事实性声明**——避免"首次""率先"等措辞，除非已从原文中直接确认该论文本身使用了此类表述。
6. **APA 7th 格式**——正文引用用 `(Author, Year)` 或 `Author (Year)`，参考文献按作者姓氏字母排序、悬挂缩进、无编号。
7. **概念解释**——应展开物理机制和技术原理，避免罗列多个技术名词或实验数据。
