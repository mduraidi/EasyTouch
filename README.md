# EasyTouch (et)

A cross-platform system automation tool supporting mouse, keyboard, screen, window, and system resource operations. Supports both CLI and MCP usage modes.

Supported platforms: Windows, Linux, macOS

## Feature Modules

### 1. Mouse Control (Mouse)

| Command | Description |
|------|------|
| `mouse_move` | Move mouse to specified coordinates (relative/absolute) |
| `mouse_click` | Mouse click (left/right/middle button, single/double click) |
| `mouse_down` | Mouse button press |
| `mouse_up` | Mouse button release |
| `mouse_scroll` | Mouse wheel scroll |
| `mouse_position` | Get current mouse position |

### 2. Keyboard Control (Keyboard)

| Command | Description |
|------|------|
| `key_press` | Press and release a key |
| `key_down` | Press a key |
| `key_up` | Release a key |
| `type_text` | Type a text string (supports Unicode) |

### 3. Screen Operations (Screen)

| Command | Description |
|------|------|
| `screenshot` | Take a screenshot |
| `pixel_color` | Get color of a specified pixel |
| `screen_list` | List all monitors |

### 4. Window Management (Window)

| Command | Description |
|------|------|
| `window_list` | List all windows |
| `window_find` | Find a window |
| `window_activate` | Activate a window |
| `window_foreground` | Get the foreground window |

### 5. System Information (System)

| Command | Description |
|------|------|
| `os_info` | Operating system information |
| `cpu_info` | CPU information |
| `memory_info` | Memory usage |
| `disk_list` | Disk list |
| `process_list` | Process list |
| `lock_screen` | Lock the screen |

### 6. Clipboard (Clipboard)

| Command | Description |
|------|------|
| `clipboard_get_text` | Get clipboard text |
| `clipboard_set_text` | Set clipboard text |
| `clipboard_clear` | Clear clipboard |
| `clipboard_get_files` | Get clipboard file list |

### 7. Audio Control (Audio)

| Command | Description |
|------|------|
| `volume_get` | Get current volume |
| `volume_set` | Set volume |
| `volume_mute` | Mute/unmute |
| `audio_devices` | List audio devices |

## CLI Mode

### Basic Usage

```bash
et <command> [options]
```

### Mouse Control

**Move Mouse**
```bash
# Absolute position
et mouse_move --x 100 --y 200

# Relative position
et mouse_move --x 50 --y -30 --relative

# Smooth move (simulates human movement)
et mouse_move --x 100 --y 200 --duration 500
```

**Mouse Click**
```bash
# Left single click
et mouse_click

# Left double click
et mouse_click --double

# Right click
et mouse_click --button right

# Middle click
et mouse_click --button middle
```

**Mouse Scroll**
```bash
# Scroll up 3 steps
et mouse_scroll --amount 3

# Scroll down 3 steps
et mouse_scroll --amount -3

# Horizontal scroll
et mouse_scroll --amount 3 --horizontal
```

**Get Mouse Position**
```bash
et mouse_position
```

### Keyboard Control

**Key Press**
```bash
# Press a single key
et key_press --key "a"
et key_press --key "enter"
et key_press --key "esc"

# Key combinations
et key_press --key "ctrl+c"
et key_press --key "ctrl+v"
et key_press --key "alt+tab"
et key_press --key "win+d"
```

**Type Text**
```bash
# Plain text
et type_text --text "Hello World"

# Unicode text
et type_text --text "Hello, World!"

# Simulate human typing (with random intervals)
et type_text --text "Hello World" --human --interval 50
```

### Screen Operations

**Screenshot**
```bash
# Full screen screenshot
et screenshot --output screenshot.png

# Region screenshot
et screenshot --x 100 --y 100 --width 800 --height 600 --output region.png

# Screenshot to clipboard (no file saved)
et screenshot
```

**Get Pixel Color**
```bash
et pixel_color --x 100 --y 200
```

**List Monitors**
```bash
et screen_list
```

### Window Management

**List Windows**
```bash
# List all visible windows
et window_list

# List all windows (including hidden)
et window_list --visible-only false

# Filter by title
et window_list --filter "Chrome"
```

**Find Window**
```bash
# Find by title
et window_find --title "Notepad"

# Find by class name
et window_find --class "Notepad"

# Find by process ID
et window_find --pid 1234
```

**Activate Window**
```bash
# Activate by title
et window_activate --title "Notepad"

# Activate by window handle
et window_activate --handle 123456
```

**Get Foreground Window**
```bash
et window_foreground
```

### System Information

**OS Information**
```bash
et os_info
```

**CPU Information**
```bash
et cpu_info
```

**Memory Information**
```bash
et memory_info
```

**Disk Information**
```bash
et disk_list
```

**Process List**
```bash
# List all processes
et process_list

# Filter by name
et process_list --filter "chrome"
```

**Lock Screen**
```bash
et lock_screen
```

### Clipboard Operations

**Get Clipboard Text**
```bash
et clipboard_get_text
```

**Set Clipboard Text**
```bash
et clipboard_set_text --text "Hello World"
```

**Clear Clipboard**
```bash
et clipboard_clear
```

**Get Clipboard File List**
```bash
et clipboard_get_files
```

### Audio Control

**Get Volume**
```bash
et volume_get
```

**Set Volume**
```bash
et volume_set --level 50
```

**Mute/Unmute**
```bash
# Mute
et volume_mute --state true

# Unmute
et volume_mute --state false
```

**List Audio Devices**
```bash
et audio_devices
```

## MCP Mode

### stdio Mode
```bash
et --mcp
```

Once started, it receives MCP protocol JSON-RPC requests via stdio.

### MCP Tools

| Tool | Description | Parameters |
|------|------|------|
| `mouse_move` | Move mouse | `x`, `y`, `relative`, `duration` |
| `mouse_click` | Click mouse | `button`, `double` |
| `mouse_position` | Get mouse position | - |
| `key_press` | Press key | `key` |
| `type_text` | Type text | `text`, `interval`, `humanLike` |
| `screenshot` | Take screenshot | `x`, `y`, `width`, `height`, `outputPath` |
| `pixel_color` | Get pixel color | `x`, `y` |
| `window_list` | List windows | `visibleOnly`, `titleFilter` |
| `window_find` | Find window | `title`, `className`, `processId` |
| `window_activate` | Activate window | `handle` |
| `system_info` | System information | - |
| `process_list` | Process list | `nameFilter` |
| `clipboard_get_text` | Get clipboard | - |
| `clipboard_set_text` | Set clipboard | `text` |
| `volume_get` | Get volume | - |
| `volume_set` | Set volume | `level` |

## Technical Specifications

- **Target Framework**: .NET 10
- **Compilation**: AOT (Ahead-of-Time)
- **Output File**: `et` / `et.exe` (single file, self-contained)
- **File Size**: ~3-5 MB (depending on platform)
- **Supported Platforms**: 
  - Windows 10/11 x64
  - Linux x64 (tested on Ubuntu, Debian, CentOS)
  - macOS x64 / ARM64 (Intel/Apple Silicon)

## Installation

### Option 1: Direct Download
Download the binary for your platform from the Releases page:
- Windows: `et.exe`
- Linux: `et`
- macOS: `et`

Place it in a directory on your system PATH or any location of your choice.

### Option 2: Build from Source

**Windows**
```bash
# Clone the repository
git clone <repository-url>
cd EasyTouch/EasyTouch-Windows

# Build (requires .NET 10 SDK)
dotnet publish EasyTouch-Windows.csproj -c Release -r win-x64 --self-contained true -p:PublishAot=true

# Output file is at bin/Release/net10.0/win-x64/publish/et.exe
```

**Linux**
```bash
# Clone the repository
git clone <repository-url>
cd EasyTouch/EasyTouch-Linux

# Build (requires .NET 10 SDK)
dotnet publish EasyTouch-Linux.csproj -c Release -r linux-x64 --self-contained true -p:PublishAot=true

# Output file is at bin/Release/net10.0/linux-x64/publish/et
# Grant execute permission
chmod +x bin/Release/net10.0/linux-x64/publish/et
```

**macOS**
```bash
# Clone the repository
git clone <repository-url>
cd EasyTouch/EasyTouch-Mac

# Build Intel version (requires .NET 10 SDK)
dotnet publish EasyTouch-Mac.csproj -c Release -r osx-x64 --self-contained true -p:PublishAot=true

# Build Apple Silicon version
dotnet publish EasyTouch-Mac.csproj -c Release -r osx-arm64 --self-contained true -p:PublishAot=true

# Output file is at bin/Release/net10.0/osx-x64/publish/et or bin/Release/net10.0/osx-arm64/publish/et
# Grant execute permission
chmod +x bin/Release/net10.0/osx-x64/publish/et
```

**Build All Platforms at Once**
```bash
# Windows
build-all.bat

# Linux / macOS
./build-all.sh
```

## MCP Client Integration

### Claude Desktop Configuration

Add the following to `claude_desktop_config.json`:

**Windows**
```json
{
  "mcpServers": {
    "easytouch": {
      "command": "C:\\path\\to\\et.exe",
      "args": ["--mcp"]
    }
  }
}
```

**Linux / macOS**
```json
{
  "mcpServers": {
    "easytouch": {
      "command": "/path/to/et",
      "args": ["--mcp"]
    }
  }
}
```

### Other MCP Clients

Configure the command as `et` (or `et.exe` on Windows), with `--mcp` as the argument, using stdio transport.

## Platform-Specific Notes

### Windows
- All features are fully supported
- Some features (such as OS shutdown, certain window operations) may require administrator privileges

### Linux
- Requires X11 display server (Wayland is not supported)
- Some features may be limited in certain desktop environments
- Recommended to run in a graphical environment

### macOS
- Accessibility permission must be granted (System Preferences -> Security & Privacy -> Accessibility)
- Screenshot feature requires Screen Recording permission
- Apple Silicon users should use the arm64 build; x64 can run via Rosetta 2

## Notes

1. **Permission Requirements**: Some features may require administrator/root privileges or accessibility permissions
2. **AOT Compilation**: The single file includes all dependencies; no .NET runtime installation is required
3. **Security**: This tool can control the system — use it only in trusted environments
4. **Compatibility**: Each platform version has been tested on its respective system

## License

MIT License
