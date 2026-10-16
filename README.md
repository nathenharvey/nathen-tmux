# tmux Cheat Sheet

A guide to `~/.tmux.conf` on **tmux 3.8**. It covers the custom bindings in this config and the built-in features worth knowing.

The prefix is **`C-a`** (Ctrl+a). Below, `prefix x` means "press `C-a`, release, then press `x`". Bindings marked **(custom)** come from this config. Everything else is a tmux default.

---

## Getting started

| Action | Command / key |
| --- | --- |
| Start a named session | `tmux new -s work` |
| Attach to a session (or create it) | `tmux new -A -s work` |
| List sessions | `tmux ls` |
| Attach to the most recent session | `tmux a` |
| Detach | `prefix d` |
| Reload the config **(custom)** | `prefix R` |
| Send a literal `C-a` to the app (e.g. start of line in the shell) | `prefix C-a` |
| Look up what a key does | `prefix /`, then press the key |
| List every binding | `prefix ?` |
| Run a tmux command | `prefix :` |

If a server was already running when this config was installed, load it once by hand with `tmux source-file ~/.tmux.conf`. After that, use `prefix R`.

---

## Sessions

| Action | Key |
| --- | --- |
| **Fast fuzzy session switcher** (new in 3.8) | `prefix S-Tab` |
| Session/window tree (with previews) | `prefix s` |
| Previous / next session | `prefix (` / `prefix )` |
| Rename session | `prefix $` |

**Switch mode** (`prefix Tab` / `prefix S-Tab`) opens a floating list. Type to fuzzy-filter, use `Up`/`Down` to move, `Enter` to jump and `Escape` to cancel. It's usually the fastest way to get anywhere.

---

## Windows

| Action | Key |
| --- | --- |
| New window, in the current directory **(custom)** | `prefix c` |
| **Fast fuzzy window switcher** (new in 3.8) | `prefix Tab` |
| Go to window 1–9 (numbering starts at 1) | `prefix 1` … `prefix 9` |
| Previous / next window (repeatable) **(custom)** | `prefix C-h` / `prefix C-l` |
| Previous / next window | `prefix p` / `prefix n` |
| Last-used window **(custom)** | `prefix a` |
| Window tree | `prefix w` |
| Find a window by its content | `prefix f` |
| Rename window | `prefix ,` |
| Kill window (asks first) | `prefix &` |
| Window menu (swap, move, respawn…) | `prefix <` |

Windows renumber automatically when you close one, so there are no gaps. "Repeatable" means you press the prefix once and can then press the key several times in a row, e.g. `C-a C-l C-l C-l`.

**Activity:** windows with new output are highlighted in **bold yellow** in the status bar. This replaces the old pop-up messages.

---

## Panes

### Splitting and moving

| Action | Key |
| --- | --- |
| Split side by side, in the current directory **(custom)** | `prefix \|` |
| Split top/bottom, in the current directory **(custom)** | `prefix -` |
| Move to the pane left/down/up/right **(custom)** | `prefix h` / `j` / `k` / `l` |
| Move with the arrow keys | `prefix ←↓↑→` |
| Last-used pane | `prefix ;` |
| Next pane | `prefix o` |
| Show pane numbers, then press one to jump | `prefix q` |
| Zoom / unzoom the pane | `prefix z` |
| Swap with the previous / next pane | `prefix {` / `prefix }` |
| Break the pane out into its own window | `prefix !` |
| Set the pane title | `prefix T` |
| Kill pane (asks first) | `prefix x` |
| Pane menu (float, tile, swap, copy, line numbers…) | `prefix >` |

### Resizing

| Action | Key |
| --- | --- |
| Resize by 5 cells, repeatable **(custom)** | `prefix H` / `J` / `K` / `L` |
| Resize by 5 cells | `prefix M-←↓↑→` |
| Resize by 1 cell | `prefix C-←↓↑→` |
| Cycle layouts | `prefix Space` |
| Spread panes evenly | `prefix E` |
| Preset layouts | `prefix M-1` … `prefix M-7` |

### Mouse

The mouse is on:

- Click a pane to focus it.
- Drag a border to resize.
- Scroll the wheel to enter copy mode and scroll back.
- Scroll over the status bar to change windows.
- **Alt+drag** a pane to move it.
- **Ctrl+drag** to draw a new floating pane (new in 3.8).
- **Right-click** for context menus.

---

## Floating and modal panes (new in 3.7/3.8)

Floating panes sit above the normal tiled layout, like a pop-up, but they are full panes.

| Action | Key |
| --- | --- |
| New floating pane | `prefix *` |
| **Modal scratch shell** in the current directory **(custom)** | `prefix S` |
| Toggle the current pane between floating and tiled | `prefix @` |
| Move/resize menu for floating panes | `prefix g` then `,` or `.` |

**Modal scratch shell (`prefix S`):** a floating shell that has focus until you close it. Press `Esc` or `C-c`, or click outside it, to close it. It's useful for quick commands without disturbing your layout.

**`prefix g` (move mode)** works on a floating pane:

| Then press | Effect |
| --- | --- |
| `1` `2` `3` `4` | Move to the top-left / top-right / bottom-left / bottom-right corner |
| `↑` `↓` `←` `→` | Move to the top / bottom / left / right edge |
| `M-1` … `M-4` | Resize to a quarter of the screen in that corner |
| `M-↑` `M-↓` `M-←` `M-→` | Resize to half the screen on that side |
| `0` | Fill the window |

**Make your own modal tool.** For example, to open lazygit in a pop-up once it's installed (`brew install lazygit`), add this to `~/.tmux.conf`:

```tmux
bind G new-pane -O -C -K -c "#{pane_current_path}" lazygit
```

`-O` makes the pane modal, `-C` closes it when you click outside, and `-K` sends every key (including `C-a`) to the program.

---

## Copy mode (vi keys)

Enter copy mode with `prefix [`, `prefix PgUp`, or by scrolling up with the mouse.

| Action | Key |
| --- | --- |
| Move | `h j k l`, `w b e`, `0 $`, `C-u C-d`, `C-b C-f` |
| Top / bottom of history | `g` / `G` |
| Top / middle / bottom of the screen | `H` / `M` / `L` |
| Search down / up | `/` / `?`, then `n` / `N` |
| Search for the word under the cursor | `*` / `#` |
| Go to a line number | `:` |
| Jump to a character | `f` `F` `t` `T` |
| Matching bracket | `%` |
| **Start a selection** **(custom)** | `v` |
| Select the whole line | `V` |
| Toggle rectangle (block) selection **(custom)** | `C-v` |
| **Copy and exit** **(custom)** | `y` (or `Enter`) |
| Paste the most recent copy | `prefix ]` |
| Choose from all paste buffers | `prefix =` |
| Refresh the view with the latest output (new in 3.8) | `r` |
| Exit | `q` |

Copied text goes to the **macOS clipboard** through OSC 52, so `Cmd-V` works in other apps. This needs a terminal that supports OSC 52 (iTerm2, Ghostty, WezTerm, kitty). In iTerm2, enable *Settings → General → Selection → Applications in terminal may access clipboard*.

**Line numbers** are shown in **hybrid** mode: the current line shows its absolute number and the others show their distance from it, as in vim with `number relativenumber`. That makes `5j` / `5k` easy to aim. Toggle them from the pane menu (`prefix >`).

**Scrollbars** appear on the right while you scroll or hover, then hide. They don't take any space from the pane. You can drag the scrollbar with the mouse.

Scrollback holds **50,000 lines** per pane.

---

## Status bar

```
 work 2 1          1:editor  2:server*  3:logs          10 Oct 19:05
 └session window pane    └ window list (centred)            └ date/time
```

Colours use 3.8's **theme colours** (`themegreen`, `themecyan`…). tmux detects whether your terminal is in light or dark mode and adjusts. To force one, run `tmux set -g theme dark` (or `light`, or `terminal` to use the terminal's own ANSI colours).

---

## Other built-in features

| Action | Key / command |
| --- | --- |
| Browse and edit every option interactively | `prefix C` (customize mode: `e` opens an editor, `C` shows only changed options) |
| List connected clients | `prefix D` |
| Recent tmux messages (useful for debugging the config) | `prefix ~` |
| Big clock | `prefix t` |
| Suspend the tmux client | `prefix C-z` |
| Show pane info | `prefix i` |

**Failed commands stay visible.** If a pane's command exits with an error (e.g. `tmux new-window 'make test'`), the pane stays open so you can read the output. Press any key to close it.

**Synchronize panes** (type into every pane in a window at once):

```
prefix :  setw synchronize-panes      # run again to turn it off
```

**Useful one-liners:**

```sh
tmux new-window -c ~/projects/foo -n foo     # named window in a directory
tmux split-window -h 'htop'                  # split running a command
tmux capture-pane -p -S - > pane.log         # dump a pane's entire history
tmux kill-session -a                         # kill every session except this one
```

---

## Reference

- Config: `~/.tmux.conf` (previous version backed up at `~/.tmux.conf.bak-2026-10-10`)
- Full manual: `man tmux`
- What changed in each release: `/opt/homebrew/opt/tmux/CHANGES`
