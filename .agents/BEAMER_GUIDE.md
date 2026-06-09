# SEU Beamer 通用编写说明

本文总结 `SEU_BeamerTemplate.tex` 的通用编写方式，供后续创建或修改 Beamer 幻灯片时复用。说明仅涉及模板结构和排版规范，与具体报告主题无关。

## 一、基本原则

- 以 `SEU_BeamerTemplate.tex` 为入口文件。
- 保留 `seu.sty`、`source/`、`fonts/` 等模板资源及其相对路径。
- 使用 XeLaTeX 编译，以支持 `xeCJK`、中文和自定义字体。
- 使用 `\section` 组织报告逻辑，使用 `frame` 创建每一页。
- 每页只表达一个核心观点，优先使用短句、图示和关键词。
- 默认沿用 `seu.sty` 定义的背景、颜色、页眉、页脚和块样式。
- 内容较多时拆页，不通过极小字号强行塞入一页。

## 二、文档骨架

```latex
\documentclass[10pt,aspectratio=43,mathserif]{beamer}

\usepackage{seu}
\usepackage{xeCJK}
\usepackage{amsmath,amsfonts,amssymb,bm}
\usepackage{color}
\usepackage{graphicx,hyperref,url}

\setsansfont[Path=fonts/]{Helvetica}
\beamertemplateballitem

\AtBeginSection[]
{
  \begin{frame}<beamer>
    \frametitle{\textbf{目录}}
    \textbf{\tableofcontents[currentsection]}
  \end{frame}
}

\title[短标题]{完整标题}
\author[短作者]{完整作者信息}
\institute[短机构]{完整机构名}
\date[\today]{\today}

\begin{document}

\begin{frame}
  \titlepage
\end{frame}

\section*{目录}
\begin{frame}
  \frametitle{\textbf{目录}}
  \textbf{\tableofcontents}
\end{frame}

\section{第一部分}
% 内容页

\section{第二部分}
% 内容页

\section*{}
% 结束页

\end{document}
```

模板默认使用 `aspectratio=43`，即 4:3 页面比例。若改为 `aspectratio=169`，需要重新检查图片尺寸和分栏宽度。

## 三、首页信息

```latex
\title[页脚短标题]{\fontsize{13pt}{18pt}\selectfont 完整报告标题}
\author[页脚短作者]{作者甲, 作者乙 \\ \medskip}
\institute[短机构名]{完整机构名}
\date[\today]{\today}
```

- 方括号中的短版本用于页眉或页脚，应保持简短。
- 花括号中的完整版本显示在标题页。
- 长标题可用 `\fontsize{13pt}{18pt}\selectfont` 调整字号和行距。
- 作者或机构需要换行时使用 `\\`。

## 四、章节与目录

```latex
\section[导航短标题]{章节完整标题}
```

- `\section{标题}`：导航栏和目录显示相同标题。
- `\section[短标题]{完整标题}`：顶部导航显示短标题，目录显示完整标题。
- `\section*{目录}` 和 `\section*{}`：创建不编号章节，适合目录页和结束页。
- 模板中的 `\AtBeginSection` 会在每个有编号章节开始时自动插入当前章节目录页。
- 章节宜控制在 3 至 6 个；标题过长或章节过多会挤占顶部导航空间。

## 五、页面 Frame

```latex
\begin{frame}
  \frametitle{\textbf{页面标题}}
  页面内容
\end{frame}
```

- 页面标题通常不超过一行。
- 每页正文建议控制在 3 至 6 个要点。
- 必要时仅在局部使用 `\small`、`\footnotesize` 或 `\scriptsize`。
- 同一章节内页面标题可以重复，但内容应有清晰递进。

## 六、常用页面版式

### 1. 项目列表

```latex
\begin{itemize}
  \item 第一条内容
  \item \textbf{关键词}：对应说明
  \item 第三条内容
\end{itemize}
```

有明确顺序时使用：

```latex
\begin{enumerate}
  \item 第一步
  \item 第二步
  \item 第三步
\end{enumerate}
```

### 2. 内容块

```latex
\begin{block}{\textbf{核心概念}}
  \begin{itemize}
    \item 要点一
    \item 要点二
  \end{itemize}
\end{block}
```

同一页通常放置一至两个块。块标题应概括内容，而不是重复页面标题。

### 3. 双栏图文

```latex
\begin{columns}
  \column{.48\textwidth}
  \footnotesize
  \begin{itemize}
    \item 左侧说明
    \item 左侧说明
  \end{itemize}

  \column{.48\textwidth}
  \begin{figure}
    \centering
    \includegraphics[width=\textwidth]{figures/example.png}
    \caption{示例图片}
    \label{fig:example}
  \end{figure}
\end{columns}
```

- 各列宽度总和应小于 `\textwidth`，为列间距留出空间。
- 常用双栏宽度为 `.48\textwidth + .48\textwidth`。
- 三图并排时，每列可使用约 `.30\textwidth`。

## 七、图片

```latex
\begin{figure}[!t]
  \centering
  \includegraphics[width=.75\textwidth]{figures/example.png}
  \caption{图片说明}
  \label{fig:example}
\end{figure}
```

- 图片建议统一存放在 `figures/` 下，并使用相对路径。
- 优先使用清晰的 PDF、PNG 或高质量 JPG。
- 图片宽度优先相对于 `\textwidth` 设置，并保持原始比例。
- `\label` 应唯一且语义明确，如 `fig:architecture`。
- 图片自身已有完整标题时，可省略 `\caption` 以节省空间。

## 八、表格

```latex
\begin{table}
  \caption{示例表格}
  \label{tab:example}
  \centering
  \footnotesize
  \begin{tabular}{|c|c|}
    \hline
    \textbf{项目} & \textbf{结果} \\
    \hline
    A & 0.80 \\
    \hline
    B & 0.90 \\
    \hline
  \end{tabular}
\end{table}
```

- 大型表格只保留关键行列。
- 表格过宽时优先精简内容，其次才考虑缩小字号。
- `\label` 推荐使用 `tab:` 前缀。

## 九、数学公式

行内公式：

```latex
策略记为 $\pi(a \mid s)$，价值函数记为 $V(s)$。
```

独立公式：

```latex
\begin{equation}
  V(s) = \mathbb{E}\left[\sum_{t=0}^{\infty}\gamma^t r_t\right].
  \label{eq:value}
\end{equation}
```

- 多行公式使用 `align`。
- 每页通常只放一个主公式，并用文字解释符号和作用。
- 向量可写为 `\bm{x}`，集合和实数域可写为 `\mathcal{X}`、`\mathbb{R}`。

## 十、中文与特殊字符

- 模板通过 `xeCJK` 支持中文，必须使用 XeLaTeX 编译。
- LaTeX 特殊字符需要转义：`\%`、`\_`、`\&`、`\#`、`\{`、`\}`。
- 英文缩写首次出现时建议给出完整名称。
- 文件名尽量使用 ASCII 字符，减少跨平台编译问题。

## 十一、结束页

```latex
\section*{}
\begin{frame}
  \begin{center}
    \begin{minipage}{1\textwidth}
      \setbeamercolor{mybox}{fg=white, bg=black!60!green}
      \begin{beamercolorbox}[wd=0.70\textwidth, rounded=true, shadow=true]{mybox}
        \LARGE \centering Thanks for Listening.
      \end{beamercolorbox}
    \end{minipage}
  \end{center}
\end{frame}
```

结束文字可以替换为“谢谢！”或“Q \& A”。结束页不需要加入有编号章节。

## 十二、模板资源与主题行为

`seu.sty` 负责以下视觉元素，通常无需在主 `.tex` 文件中修改：

- 东南大学背景图、校名图和校徽。
- 顶部章节导航栏。
- 底部作者、日期、短标题和页码。
- 绿色主题色、圆角块和阴影。
- 图片与表格标题的中文名称及编号。

移动或复制项目时，应同时保留：

```text
seu.sty
source/seu_background.png
source/seu_logo.png
source/seu_title.png
fonts/
```

## 十三、编译方式

项目 `Makefile` 使用 XeLaTeX，并连续编译两次以更新目录、导航和引用：

```bash
make SEU_BeamerTemplate
```

也可手动运行：

```bash
xelatex SEU_BeamerTemplate.tex
xelatex SEU_BeamerTemplate.tex
```

清理辅助文件：

```bash
make clean
```

新建其他入口文件时，需要修改 `Makefile`，或直接对新文件运行两次 `xelatex`。

## 十四、推荐制作流程

1. 复制模板入口文件，并保留导言区、标题页、目录页和结束页。
2. 填写标题、作者、机构和日期。
3. 先确定 3 至 6 个章节，再填写 `\section`。
4. 为每页写一句核心结论，据此确定页面标题。
5. 选择列表、内容块、双栏、表格或公式等合适版式。
6. 添加图片与引用，检查路径和标签唯一性。
7. 使用 XeLaTeX 连续编译两次。
8. 逐页检查溢出、字号、图片清晰度、导航和页码。

## 十五、检查清单

- [ ] 使用 XeLaTeX 编译且无报错。
- [ ] 标题页信息完整，短标题和短作者适合页脚显示。
- [ ] 章节数量适中，顶部导航未拥挤。
- [ ] 每页有明确标题和单一核心观点。
- [ ] 页面文字精简，无大段正文。
- [ ] 图片路径有效、清晰且未变形。
- [ ] 表格已精简，字号可以正常阅读。
- [ ] 公式尺寸合理，并有必要的文字解释。
- [ ] `%`、`_`、`&` 等特殊字符已正确转义。
- [ ] 无内容越界或底部遮挡。
- [ ] 连续编译两次后，目录、导航、页码和引用正确。
- [ ] 未无意修改 `seu.sty` 或模板资源。
