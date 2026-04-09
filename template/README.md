# 数学错题本模板

这个模板用于把数学错题整理成 LaTeX，并最终输出为适合平板二刷的 PDF。

模板默认采用 A4 横版，题目页预留了更大的手写区域，方便在平板上直接书写。

## 结构

```text
template/
  main.tex
  preamble.sty
  README.md
  problems/
    YYYY-MM-DD-XX.tex
```

- `main.tex`：总入口文件，从这里编译 PDF。
- `preamble.sty`：统一样式、颜色、页眉页脚和横版页面设置。
- `problems/*.tex`：每道题一个文件，按日期编号命名。

## 题目组织规则

每道题固定两页：

1. 第 1 页：题目 + 答题区
2. 第 2 页：正确解法 + 知识点相关信息

备注会显示在两页页脚右下角。

总文件默认不额外插入封面页，这样每道题都严格保持两页结构。

## 文件命名

推荐按自然排序命名：

```text
2026-04-09-01.tex
2026-04-09-02.tex
2026-04-10-01.tex
```

## 新增一道错题

在 `problems/` 下新建一个文件，例如 `2026-04-10-01.tex`，内容如下：

```latex
\begin{mistake}{2026-04-10}{01}
\mistakeremarktext{这里写备注}

\begin{problemcontent}
这里写题目
\end{problemcontent}

\begin{solutioncontent}
这里写正确解法

\mistakeknowledge{这里写知识点相关信息}
\end{solutioncontent}
\end{mistake}
```

这种环境式写法比六参数宏更适合复杂内容；题目、解法里可以直接放多段文字、列表和多行公式。

多行公式请统一写成 `\[ ... \begin{aligned} ... \end{aligned} ... \]`，不要使用 `align`、`align*`、`equation`、`equation*` 这类环境，以免嵌套时报错。

然后在 `main.tex` 中按顺序加入：

```latex
\input{problems/2026-04-09-01.tex}
\input{problems/2026-04-10-01.tex}
```

## 编译方式

推荐使用 `XeLaTeX`：

```bash
xelatex main.tex
```

如果你希望交叉引用或页码更稳定，可以连续编译两次：

```bash
xelatex main.tex
xelatex main.tex
```

## 字体说明

模板默认使用以下字体：

- 西文：`TeX Gyre Pagella`
- 中文：由 `ctex` 自动处理，通常使用 TeX Live 自带的 Fandol 字体

如果你想换成系统里的其它中文字体，也可以在 `preamble.sty` 中自行指定。

## 调整建议

- 想增加手写空间：修改 `preamble.sty` 中 `\reviewspace` 里的高度比例。
- 想调整颜色：修改 `MistakeAccent`、`MistakeLine`、`MistakeMuted` 三个颜色值。
- 想改变页面方向或页边距：修改 `geometry` 的 `landscape` 和 `margin=20mm`。
