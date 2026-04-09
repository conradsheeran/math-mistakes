---
name: math-mistakes
description: Manage math mistake notebook — add problems from pasted LaTeX or export/archive the notebook as PDF
---

# Math Mistakes Workflow

This skill defines two workflows for managing the math mistake notebook.

## Workflow A: Add Mistakes

### Trigger

User pastes LaTeX code in the chat containing one or more `\begin{mistake}...\end{mistake}` blocks.

### Steps

For each `\begin{mistake}...\end{mistake}` block found in the pasted content:

1. **Determine filename**
   - Format: `YYYY-MM-DD-XX.tex` where `YYYY-MM-DD` is today's date and `XX` is a zero-padded sequence number
   - Check existing files in `problems/` for the same date and continue numbering from the highest existing number
   - Example: if `2026-04-10-01.tex` and `2026-04-10-02.tex` exist, the next file is `2026-04-10-03.tex`

2. **Add parameters to `\begin{mistake}`**
   - The pasted code may have `\begin{mistake}` without arguments
   - Rewrite to `\begin{mistake}{YYYY-MM-DD}{XX}` matching the filename

3. **Fix LaTeX formatting**
   - Convert `align*`, `align`, `equation*`, `equation` environments to `\[ ... \begin{aligned} ... \end{aligned} ... \]`
   - Preserve all content inside the environments — only change the wrapping
   - Do NOT modify any math content, solutions, or knowledge sections

4. **Write the file**
   - Save to `problems/YYYY-MM-DD-XX.tex`

5. **Update `main.tex`**
   - Insert `\input{problems/YYYY-MM-DD-XX.tex}` on a new line immediately before `\end{document}`

6. **Atomic git commit**
   - Stage both `problems/YYYY-MM-DD-XX.tex` and `main.tex`
   - Commit message: `feat(problems): add mistake YYYY-MM-DD-XX`

Repeat steps 1–6 for each mistake block. Each mistake gets its own commit.

### Example

User pastes:

```latex
\begin{mistake}

\begin{problemcontent}
Some problem here.
\end{problemcontent}

\begin{solutioncontent}
Some solution here.
\end{solutioncontent}

\end{mistake}
```

AI creates `problems/2026-04-10-01.tex`:

```latex
\begin{mistake}{2026-04-10}{01}

\begin{problemcontent}
Some problem here.
\end{problemcontent}

\begin{solutioncontent}
Some solution here.
\end{solutioncontent}

\end{mistake}
```

And adds `\input{problems/2026-04-10-01.tex}` to `main.tex`.

---

## Workflow B: Export Mistakes

### Trigger

User asks to export, generate PDF, or produce the current notebook.

### Steps

1. **Count problems**
   - Count `.tex` files in `problems/` (exclude `.gitkeep`)
   - If count < 10:
     - Warn user: "当前错题本只有 N 道题，建议积累至少 10 道后再导出。是否仍然继续？"
     - If user declines → stop
     - If user insists → continue

2. **Compile PDF**
   - Run `xelatex main.tex` twice from the repository root directory (two passes ensure stable page numbers)
   - Verify `main.pdf` was generated successfully

3. **Create archive directory**
   - Directory: `archive/YYYY-MM-DD/` (today's date)
   - If directory already exists (same-day export), append a sequence number: `archive/YYYY-MM-DD-2/`

4. **Move problem files to archive**
   - Move all `.tex` files from `problems/` to `archive/YYYY-MM-DD/`
   - Do NOT move `.gitkeep`

5. **Move PDF to archive**
   - Move `main.pdf` to `archive/YYYY-MM-DD/YYYY-MM-DD.pdf`

6. **Atomic git commit**
   - Stage `archive/YYYY-MM-DD/` (all archived files and PDF)
   - Stage the removal of files from `problems/`
   - Commit message: `feat(archive): export mistake batch YYYY-MM-DD`

7. **Reset notebook**
   - Remove all `\input{problems/...}` lines from `main.tex` (keep the document skeleton)
   - Ensure `problems/.gitkeep` still exists

8. **Atomic git commit**
   - Stage `main.tex`
   - Commit message: `chore(problems): reset notebook after export`

### Post-export state

After export, `main.tex` should look like:

```latex
\documentclass[12pt]{article}

\usepackage{template/preamble}

\title{数学错题本}
\author{}
\date{}

\begin{document}

\end{document}
```

And `problems/` should contain only `.gitkeep`.
