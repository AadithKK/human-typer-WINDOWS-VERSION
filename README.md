# Human Typer — Windows 11 Version

> **This is a Windows 11 fork** of the original [Human Typer](https://github.com/aadithkk/human-typer) project.
> The original was built for Linux/macOS. This fork rewrites the keyboard input system to use the native Windows `SendInput` API so it works reliably on Windows 10 and Windows 11.

---

## What Changed from the Original

| Feature | Original | This Fork |
|---|---|---|
| Platform | Linux / macOS | **Windows 10 / 11** |
| Keyboard input | `pyautogui` type | Windows `SendInput` (Unicode-safe) |
| Background typing | Not available | Works via `ctypes.windll.user32.SendInput` |
| Hotkeys | xdotool-based | `pynput` global listeners |

The core typing engine was rewritten to use `ctypes` + `SendInput` instead of `pyautogui.typewrite`, which gave broken results on Windows for special characters and non-ASCII text.

---

## What It Does

Human Typer takes any text you paste in and types it out character-by-character as if a real person is typing — complete with randomized delays, typos, self-corrections, and natural rhythm. Useful for demos, presentations, or anywhere you need text to appear as if it's being typed live.

---

## Requirements

- **Windows 10 or Windows 11** (64-bit)
- **Python 3.10+**

### Python Dependencies

```
pyautogui
customtkinter
Pillow
pynput
```

Install them all at once:

```bash
pip install pyautogui customtkinter Pillow pynput
```

> Note: `ctypes` is built into Python on Windows — no extra install needed.

---

## How to Run

1. **Clone or download** this repository:

   ```bash
   git clone https://github.com/aadithkk/human-typer-windows-version.git
   cd human-typer-windows-version
   ```

2. **Install dependencies:**

   ```bash
   pip install pyautogui customtkinter Pillow pynput
   ```

3. **Run the app:**

   ```bash
   python human_typer.py
   ```

   The GUI window will open.

---

## How to Use

### Basic Steps

1. **Paste your text** into the large text box on the main screen.
2. **Adjust settings** (speed, typo rate, chunk mode, etc.) using the Settings page.
3. **Click Start** or press `Ctrl + Alt + H`.
4. **Switch to your target window** (e.g., a browser, Word, Notepad) — the app will type the text there automatically.

### Global Hotkeys

These work even when the Human Typer window is not focused:

| Hotkey | Action |
|---|---|
| `Ctrl + Alt + H` | Start typing |
| `Ctrl + Alt + S` | Stop typing immediately |
| `Ctrl + Alt + Space` | Resume next chunk (when Chunk Mode is paused) |

### Settings Explained

| Setting | What it does |
|---|---|
| **WPM / Speed** | Controls how fast characters are typed |
| **Typo Rate** | How often it makes a random typo and self-corrects |
| **Case Slip** | Occasionally types a letter in wrong case (like hitting Shift too long), then fixes it |
| **Chunk Mode** | Pauses every few words and waits for `Ctrl+Alt+Space` before continuing — simulates reading as you type |

---

## Claude Code Integration (HTTP API)

The app runs a local HTTP server on port **7799** so Claude Code (or any script) can push text to it directly.

### Endpoints

| Method | Path | Description |
|---|---|---|
| `POST` | `/type` | Load text into the text box |
| `POST` | `/type-and-start` | Load text and immediately start typing |
| `GET` | `/status` | Check if Human Typer is running |

### Example

```bash
# Load text
curl -X POST http://127.0.0.1:7799/type -d "Hello, this is a test."

# Load text and auto-start typing
curl -X POST http://127.0.0.1:7799/type-and-start -d "Hello, this is a test."

# Check status
curl http://127.0.0.1:7799/status
```

---

## Troubleshooting

**Text types in the wrong window**
- Make sure you click into your target window after pressing Start (or using the hotkey). The app types into whichever window has focus.

**Nothing types at all**
- Try running `python human_typer.py` as Administrator. Some apps (like UAC dialogs or elevated windows) block `SendInput` from non-elevated processes.

**`pynput` hotkeys not working**
- This is usually an antivirus or security software blocking keyboard hooks. Add an exception for Python or run as Administrator.

**`ModuleNotFoundError`**
- Run `pip install pyautogui customtkinter Pillow pynput` and make sure you're using the correct Python environment.

---

## Original Project

This fork is based on the original Human Typer by [@aadithkk](https://github.com/aadithkk).
Check out the original for the Linux/macOS version.

---

## License

Same license as the original project. See the original repository for details.
