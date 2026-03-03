# mdview

> Ever tried to read a Markdown file in the terminal and ended up staring at a wall of `##`, `**`, and backticks? You open it with `cat` and it's unreadable. You try `less` and it's still raw source. You don't have a GUI, you're SSH'd into a server, and you just want to read the damn file.

`mdview` fixes that. One command, fully rendered Markdown, in any terminal, on any Linux distro.

---

A terminal Markdown viewer with syntax highlighting, line numbers, and search.

Renders Markdown beautifully in your terminal using [rich](https://github.com/Textualize/rich), then pipes it through `less` for scrolling and search.

## Features

- Rendered Markdown (headings, bold, tables, code blocks) — not raw source
- Line numbers in the gutter
- Scroll with arrow keys / Page Up / Page Down
- Search with `/pattern` (case-insensitive)
- Works as a pipe: `cat file.md | mdview` strips colors for clean output

## Requirements

- Python 3.7+
- `less` (pre-installed on macOS and most Linux distros)
- [rich](https://github.com/Textualize/rich)

```bash
pip install rich
```

## Installation

```bash
curl -o ~/.local/bin/mdview https://raw.githubusercontent.com/YOUR_USERNAME/mdview/main/mdview
chmod +x ~/.local/bin/mdview
```

Or manually:

```bash
cp mdview ~/.local/bin/mdview
chmod +x ~/.local/bin/mdview
```

Make sure `~/.local/bin` is in your `PATH`:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## Usage

```bash
mdview file.md
```

### Keyboard shortcuts (inside the viewer)

| Key | Action |
|---|---|
| `↑` / `↓` | Scroll line by line |
| `PgUp` / `PgDn` | Scroll page by page |
| `g` / `G` | Jump to top / bottom |
| `/pattern` | Search forward |
| `n` / `N` | Next / previous match |
| `q` | Quit |

### Pipe mode (no colors, plain text)

```bash
mdview file.md | grep "something"
mdview file.md > output.txt
```

## How it works

1. Renders the Markdown with `rich.Markdown` into a buffer with ANSI color codes
2. Shrinks the render width by 8 columns to leave room for `less -N`'s line number gutter
3. Pipes the result to `less -NRi`:
   - `-N` — line numbers
   - `-R` — pass ANSI color codes through to the terminal
   - `-i` — case-insensitive search
4. When stdout is not a terminal (pipe/redirect), writes plain rendered output directly
