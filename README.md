# ⌨️ Cross-Platform Keymap — Reduce Hand Movement While Typing

A keymap and cross-platform setup guide to remap your keyboard so that symbols, navigation, numpad, and even mouse control are all accessible from the home rows — without reaching for other keys.

It works by using `capslock`, `,`, and `.` as trigger keys. Hold one down, and the letter keys temporarily turn into symbols, arrow keys, numpad, or mouse controls. Standard Ctrl/Shift/Alt/Win combos are untouched.

This repo provides the config files and instructions to set this up using **Kanata**, **AutoHotkey**, **Karabiner**, or an included **Python script** — pick whichever fits your OS and workflow.

### Core Remappings (Global)

![Remappings for comma and capslock triggers](images/comma_caps_period_layers.jpg)

---

## 🟩 Installation

Available for Windows (AutoHotkey), macOS (Kanata), and Linux (Kanata or Python).

---

### 🍎 macOS: Kanata (Recommended)

**Best for:** Native performance and deep system integration.

This uses **Kanata** to run the keyboard layers. It requires the **Karabiner Driver** to inject keystrokes, but runs independently of the Karabiner app itself.

#### 1. Prerequisites: The Driver

1. Download and install [Karabiner-Elements](https://karabiner-elements.pqrs.org/).
2. Open the app once to ensure the extension (`Karabiner-DriverKit-VirtualHIDDevice`) is approved in **System Settings > Privacy & Security**.
3. **Critical:** Fully quit Karabiner-Elements (Cmd+Q) and make sure it isn't running in the menu bar. Only the driver needs to stay installed — not the app.

#### 2. Quick Setup (Terminal)

Open a terminal inside the `mac_kanata` folder of this repo and run:

```bash
# 1. Install Kanata
curl -L -o kanata https://github.com/jtroo/kanata/releases/latest/download/kanata_macos_arm64
chmod +x kanata
sudo mv kanata /usr/local/bin/

# 2. Set up the config directory
mkdir -p ~/.config/kanata

# 3. Copy the config file into place
cp config.kbd ~/.config/kanata/config.kbd

# 4. Run Kanata
sudo kanata --cfg ~/.config/kanata/config.kbd
```

*Note: macOS will prompt for "Input Monitoring" permissions on first run. Allow it for Terminal (or `kanata`), then re-run the command.*

#### 3. Controls & Layers

- **Top row default:** Media/Special keys (Brightness, Spotlight, Mission Control, Volume, etc.) — F5/F6 stay as standard function keys.
- **Hold CapsLock:** Top row temporarily becomes standard F1–F12.
- **Caps Lock logic:**
  - Tap (<200ms): toggles normal Caps Lock.
  - Hold (>200ms) or combo: activates the Symbols Layer and switches the top row to F-keys.

#### 4. Troubleshooting

**"Exclusive access and device already open"** — Karabiner's app is still running and fighting Kanata for control. Force-kill it:

```bash
sudo pkill -9 -f karabiner
```

**Updating the config** — Edit `config.kbd`, press `Ctrl+C` to stop Kanata, then re-run the start command to reload.

**Force quit** — `LeftCtrl + Space + Esc` or `Ctrl+C` instantly kills Kanata and restores your normal keyboard.

---

### 🐧 Linux Method 1: Kanata (Recommended)

**Best for:** Performance, gaming, and online assessments (runs at the kernel level, undetectable by userspace apps).

Paste this into your terminal — it downloads the binary and config to `~/kanata` and runs it immediately, no git clone needed:

```bash
# 1. Create a folder for Kanata and the config
mkdir -p ~/kanata

# 2. Download the binary and config file
wget https://github.com/jtroo/kanata/releases/download/v1.6.1/kanata -O ~/kanata/kanata
wget https://raw.githubusercontent.com/riteshrajd/AHK/main/kanata/config.kbd -O ~/kanata/config.kbd
chmod +x ~/kanata/kanata

# 3. Set up uinput permissions (lets Kanata create a virtual keyboard)
sudo groupadd uinput
sudo usermod -aG uinput $USER
echo 'KERNEL=="uinput", MODE="0660", GROUP="uinput", OPTIONS+="static_node=uinput"' | sudo tee /etc/udev/rules.d/99-input.rules
sudo udevadm control --reload-rules && sudo udevadm trigger

# 4. Run it (keep this terminal open while active)
sudo ~/kanata/kanata -c ~/kanata/config.kbd
```

**To stop:** `Ctrl+C` in the running terminal, or from a new tab: `sudo pkill kanata`

---

### 🐧 Linux Method 2: Python Script (Legacy)

Uses the `evdev` library to intercept and inject input events. Good for quick testing without downloading binaries.

1. **Install the dependency:**
   ```bash
   # Fedora
   sudo dnf install python3-evdev
   # or via pip
   sudo pip install evdev
   ```
2. **Run the script** (must be run as root to grab the input device):
   ```bash
   sudo python3 key_remapper.py
   ```
3. **(Optional) Run on startup:** create a systemd service or add a sudo-enabled startup command.

---

### 🪟 Windows

1. Install [AutoHotkey](https://www.autohotkey.com/).
2. Download the `.ahk` files and run them in this order:
   1. `, . caps.ahk`
   2. `num_pad_handling.ahk`

You're set.

---

## ❓ How to Disable / Toggle

**Linux (Kanata):** `Ctrl+C` in the running terminal, or `sudo pkill kanata`

**Linux (Python):** `Ctrl+C` in the terminal, or `sudo pkill -f key_remapper.py`

**Windows:** Right-click the green 'H' icon in the system tray, then "Pause Script" or "Exit."

---

## 🔵 Remappings

`,`, `.`, and `capslock` are trigger keys — hold one down and the alphabet rows remap to new functions.

- **To use Caps Lock normally:** press it twice within 0.2 seconds.

### 🖱️ Mouse Mode (Linux Python Only)

*Currently only available in the Python version.*

- **Toggle ON/OFF:** hold `.`, tap `u`, release both.

**While active:**

| Function | Key | Description |
|---|---|---|
| Movement | `e` `s` `d` `f` | Up / Left / Down / Right |
| Left Click | `j` | |
| Right Click | `k` | |
| Scrolling | Hold `m` | Turns `e/s/d/f` into Scroll Up/Left/Down/Right |
| Speed: Normal | (default) | |
| Speed: Medium | Hold `.` | |
| Speed: Fast | Hold `Space` | |

Current settings (in `key_remapper.py`):
```
MOUSE_SPEED_NORMAL = 1
MOUSE_SPEED_MEDIUM = 12    # Dot held
MOUSE_SPEED_FAST = 25      # Space held

SCROLL_SPEED_NORMAL = 0.2
SCROLL_SPEED_MEDIUM = 3    # Dot held
SCROLL_SPEED_FAST = 10     # Space held
```

### Core Remappings (Global)

#### Hold `Capslock` — Symbols
`y`→`$` `u`→`>` `i`→`=` `o`→`\` `p`→`!`
`h`→`<` `j`→`(` `k`→`+` `l`→`*` `;`→`-`
`n`→`_` `m`→`)` `[`→`?`

#### Hold `,` — Symbols
`q`→`~` `w`→`.` `e`→`,` `r`→`&` `t`→`^`
`a`→`@` `s`→`%` `d`→`{` `f`→`[` `g`→`'`
`z`→`#` `x`→`` ` `` `c`→`}` `v`→`]` `b`→`|`

#### Hold `.` — Navigation & Editing
`k`→Enter · `j`→Backspace · `i`→Shift+Enter · `m`→Ctrl+Backspace (delete word) · `n`→Delete
`e`→Up · `d`→Down · `s`→Left · `f`→Right
`r`→Ctrl+Right (next word) · `w`→Ctrl+Left (previous word)

#### Hold `.` then `,` — Numpad
*Hold `.` first, then also hold `,` to activate.*
`w`→`7` `e`→`8` `r`→`9`
`s`→`4` `d`→`5` `f`→`6` `g`→`0`
`x`→`1` `c`→`2` `v`→`3`
`a`→`,` `z`→`.`

### ✨ Linux Extras
- `Capslock` + `f` → Alt+Tab (App Switcher)
- `Capslock` + `d` → Alt+Shift+Tab (App Switcher Reverse)
- `.` + `h` → Delete Whole Line