# EasyTouch Linux MCP Skill

## Basic Information

- **Name**: EasyTouch Linux Automation
- **Version**: 1.0.0
- **Description**: Linux system automation tool supporting mouse, keyboard, screen, window, and system resource operations
- **Author**: MaomiAgent Team
- **License**: MIT

## Supported Platforms

- Ubuntu 20.04+
- Debian 10+
- CentOS 8+
- Fedora 32+
- Arch Linux
- Other distributions supporting X11 or Wayland

## Dependencies

### X11 Environment
- `xdotool` - Mouse and keyboard control
- `xclip` or `xsel` - Clipboard operations
- `scrot` or `gnome-screenshot` - Screenshots
- `wmctrl` - Window management

### Wayland Environment
- `ydotool` - Mouse and keyboard control
- `wl-clipboard` - Clipboard operations
- `grim` - Screenshots

### Audio Control
- `alsa-utils` (amixer) or `pulseaudio-utils` (pactl) or `wireplumber` (wpctl)

Install dependencies:
```bash
# Debian/Ubuntu
sudo apt install xdotool xclip scrot wmctrl alsa-utils

# Or Wayland
sudo apt install ydotool wl-clipboard grim

# Fedora
sudo dnf install xdotool xclip scrot wmctrl alsa-utils

# Arch
sudo pacman -S xdotool xclip scrot wmctrl alsa-utils
```

## Features

### 1. Mouse Control
- Move mouse to specified coordinates (supports X11 and Wayland)
- Mouse click (left/right/middle button, supports double click)
- Mouse press/release
- Mouse wheel scroll
- Get current mouse position

### 2. Keyboard Control
- Key press and release
- Key combination operations (Ctrl+C, Alt+Tab, etc.)
- Text input (supports multiple languages)
- Simulated human typing

### 3. Screen Operations
- Full screen/region screenshot
- Get color of a specified pixel
- List all monitors

### 4. Window Management
- List all windows
- Find window by title
- Activate window (X11 only)
- Get foreground window

### 5. System Information
- Get operating system information
- CPU information and usage
- Memory usage
- Disk information
- Process list

### 6. Clipboard Operations
- Get/set clipboard text
- Get clipboard file list
- Clear clipboard

### 7. Audio Control
- Get/set system volume
- Mute/unmute
- List audio devices

## Usage

### CLI Mode

```bash
# Mouse operations
et mouse_move --x 100 --y 200
et mouse_click --button left --double
et mouse_position

# Keyboard operations
et key_press --key "ctrl+c"
et type_text --text "Hello World"

# Screenshot
et screenshot --output screenshot.png
et pixel_color --x 100 --y 100

# Window management
et window_list
et window_activate --title "Firefox"

# System information
et os_info
et cpu_info
et memory_info
et process_list

# Clipboard
et clipboard_get_text
et clipboard_set_text --text "Hello"

# Audio
et volume_set --level 50
et volume_mute --state true
```

### MCP Mode

```bash
et --mcp
```

## MCP Tools

| Tool | Description | Parameters |
|------|------|------|
| `mouse_move` | Move mouse | `x`, `y`, `relative`, `duration` |
| `mouse_click` | Click mouse | `button`, `double` |
| `mouse_down` | Mouse press | `button` |
| `mouse_up` | Mouse release | `button` |
| `mouse_scroll` | Mouse wheel | `amount`, `horizontal` |
| `mouse_position` | Get mouse position | - |
| `key_press` | Press key | `key` |
| `key_down` | Key press | `key` |
| `key_up` | Key release | `key` |
| `type_text` | Type text | `text`, `interval`, `humanLike` |
| `screenshot` | Take screenshot | `x`, `y`, `width`, `height`, `outputPath` |
| `pixel_color` | Get pixel color | `x`, `y` |
| `screen_list` | List monitors | - |
| `window_list` | List windows | `visibleOnly`, `titleFilter` |
| `window_find` | Find window | `title`, `className`, `processId` |
| `window_activate` | Activate window | `handle` |
| `window_foreground` | Get foreground window | - |
| `os_info` | OS information | - |
| `cpu_info` | CPU information | - |
| `memory_info` | Memory information | - |
| `disk_list` | Disk list | - |
| `process_list` | Process list | `nameFilter` |
| `lock_screen` | Lock screen | - |
| `clipboard_get_text` | Get clipboard text | - |
| `clipboard_set_text` | Set clipboard text | `text` |
| `clipboard_clear` | Clear clipboard | - |
| `clipboard_get_files` | Get clipboard files | - |
| `volume_get` | Get volume | - |
| `volume_set` | Set volume | `level` |
| `volume_mute` | Mute/unmute | `state` |
| `audio_devices` | List audio devices | - |

## Technical Specifications

- **Target Framework**: .NET 10
- **Compilation**: AOT (Ahead-of-Time)
- **Output File**: `et` (single file, self-contained)
- **File Size**: ~4 MB
- **Platform**: Linux x64 (glibc 2.17+)

## Installation

### Option 1: Direct Download
Download the `et` file and place it in a directory on your system PATH:
```bash
sudo cp et /usr/local/bin/
sudo chmod +x /usr/local/bin/et
```

### Option 2: Build from Source

```bash
# Clone the repository
cd tools/EasyTouch/EasyTouch-Linux

# Build (requires .NET 10 SDK)
dotnet publish -c Release -r linux-x64 --self-contained true -p:PublishAot=true

# Output file is at bin/Release/net10.0/linux-x64/publish/et
```

## MCP Client Integration

### Claude Desktop Configuration

Add the following to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "easytouch": {
      "command": "/usr/local/bin/et",
      "args": ["--mcp"]
    }
  }
}
```

### Other MCP Clients

Configure the command as `et`, with `--mcp` as the argument, using stdio transport.

## Notes

1. **X11 vs Wayland**: Wayland's security model restricts some features (such as window activation); the tool automatically detects and uses the appropriate method
2. **Permissions**: Some features may require user permissions; running as a regular user is recommended
3. **Dependencies**: Ensure the required system tools are installed (xdotool/xclip, etc.)
4. **Compatibility**: Some features are limited in a pure Wayland environment

## Known Limitations

- **Wayland Window Management**: Due to security restrictions, Wayland does not support directly enumerating and activating other applications' windows
- **Global Shortcuts**: Cannot simulate global shortcuts via this tool (requires window manager support)
- **Screen Recording Permissions**: macOS-style screen recording permission management may affect some distributions

## Troubleshooting

### Command Not Responding
- Check that dependencies are installed: `which xdotool`
- Check permissions: ensure the current user has access to X11/Wayland

### Screenshot Fails
- Install screenshot tool: `sudo apt install scrot` or `sudo apt install grim`
- Check file permissions: ensure the output directory is writable

### Audio Control Fails
- Check audio system: `pactl info` or `amixer`
- Ensure the user is in the audio group: `sudo usermod -a -G audio $USER`
