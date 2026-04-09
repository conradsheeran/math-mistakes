# Math Mistakes Project

## Git Convention

This project uses [Conventional Commits](https://www.conventionalcommits.org/) in English.

### Format

```
type(scope): description
```

### Types

| Type | Usage |
|---|---|
| `feat` | New feature (add problems, new functionality) |
| `fix` | Bug fix |
| `style` | Visual/style changes (colors, layout, spacing) |
| `refactor` | Code restructuring without behavior change |
| `docs` | Documentation changes |
| `chore` | Maintenance (gitignore, dependencies, cleanup) |

### Scopes

| Scope | Usage |
|---|---|
| `problems` | Changes to problem files in `problems/` |
| `template` | Changes to template structure in `template/` |
| `archive` | Archiving operations |
| `skill` | Skill-related changes |

Scope is optional but recommended when the change clearly belongs to one area.

## LaTeX Conventions

- Multi-line math: use `\[ ... \begin{aligned} ... \end{aligned} ... \]`
- Do NOT use `align`, `align*`, `equation`, or `equation*` environments in problem files — they cause nesting errors
- Each problem file: `problems/YYYY-MM-DD-XX.tex`
- Compile with `xelatex main.tex` from the repository root

## Workflow

The primary workflow skill is defined in `skill/math-mistakes.md`. Reference it when:
- User pastes LaTeX code containing `\begin{mistake}` blocks
- User asks to export or generate PDF from the current notebook

## Project Structure

```
main.tex              → active notebook entry point
problems/             → active problem .tex files
template/             → template and examples (reference only)
archive/              → exported batches (created on first export)
skill/                → workflow skill definitions
ref/                  → reference LaTeX class files
```
