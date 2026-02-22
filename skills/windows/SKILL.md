# EasyTouch Windows MCP Skill

## Basic Information

- **Name**: EasyTouch Windows Automation
- **Version**: 1.0.0
- **Description**: Windows system automation tool supporting mouse, keyboard, screen, window, and system resource operations
- **Author**: Chizhe Gongliang
- **License**: MIT

## Features

### 1. Mouse Control
- Move mouse to specified coordinates (supports relative/absolute position, smooth movement)
- Mouse click (left/right/middle button, supports double click)
- Mouse press/release
- Mouse wheel scroll
- Get current mouse position

### 2. Keyboard Control
- Key press and release
- Key combination operations
- Text input (supports Unicode and simulated human typing)
- Independent key_down/key_up operations

### 3. Screen Operations
- Full screen/region screenshot (PNG format)
- Get color of a specified pixel
- List all monitors

### 4. Window Management
- List all windows
- Find window by title/class name/PID
- Activate/minimize/maximize/close windows
- Get foreground window
- Set window always-on-top

### 5. System Information
- Get operating system information
- CPU information and usage
- Memory usage
- Disk information
- Process list and management

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
et window_activate --handle 123456

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

Once started, it receives MCP protocol JSON-RPC requests via stdio.

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
| `window_activate` | Activate window | `title`, `handle` |
| `window_foreground` | Get foreground window | - |
| `window_minimize` | Minimize window | `handle` |
| `window_maximize` | Maximize window | `handle` |
| `window_close` | Close window | `handle` |
| `window_set_topmost` | Set window always-on-top | `handle`, `topmost` |
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
- **Output File**: `et.exe` (single file, self-contained)
- **File Size**: ~3.9 MB
- **Platform**: Windows 10/11 x64

## Installation

### Option 1: Direct Download
Download `et.exe` and place it in a directory on your system PATH or any location of your choice.

### Option 2: Build from Source

```bash
# Clone the repository
git clone <repository-url>
cd tools/EasyTouch/EasyTouch-Windows

# Build (requires .NET 10 SDK)
dotnet publish EasyTouch-Windows.csproj -c Release -r win-x64 --self-contained true -p:PublishAot=true

# Output file is at bin/Release/net10.0/win-x64/publish/et.exe
```

## MCP Client Integration

### Claude Desktop Configuration

Add the following to `claude_desktop_config.json`:

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

### Other MCP Clients

Configure the command as `et.exe`, with `--mcp` as the argument, using stdio transport.

## Notes

1. **Administrator Privileges**: Some features (such as OS shutdown, certain window operations) may require administrator privileges
2. **AOT Compilation**: The single file includes all dependencies; no .NET runtime installation is required
3. **Security**: This tool can control the system — use it only in trusted environments
4. **Compatibility**: Tested on Windows 10/11 x64 only
