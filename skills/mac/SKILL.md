# EasyTouch macOS MCP Skill

## Basic Information

- **Name**: EasyTouch macOS Automation
- **Version**: 1.0.0
- **Description**: macOS system automation tool supporting mouse, keyboard, screen, window, and system resource operations
- **Author**: MaomiAgent Team
- **License**: MIT

## Supported Platforms

- macOS 11 (Big Sur) +
- macOS 12 (Monterey)
- macOS 13 (Ventura)
- macOS 14 (Sonoma)
- macOS 15 (Sequoia)
- Intel and Apple Silicon (Rosetta 2)

## Dependencies

This tool uses AppleScript and system commands; no additional dependencies are required.

**System Requirements**:
- macOS 11 or later
- Accessibility permission (the system will prompt on first use)

## Features

### 1. Mouse Control
- Move mouse to specified coordinates
- Mouse click (left/right/middle button, supports double click)
- Mouse press/release
- Mouse wheel scroll
- Get current mouse position

### 2. Keyboard Control
- Key press and release
- Key combination operations (⌘+C, ⌥+Tab, etc.)
- Text input (supports multiple languages)
- Simulated human typing

### 3. Screen Operations
- Full screen/region screenshot
- Get color of a specified pixel
- List all monitors

### 4. Window Management
- List all windows
- Find window by title
- Activate window
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
et key_press --key "command+c"
et type_text --text "Hello World"

# Screenshot
et screenshot --output screenshot.png
et pixel_color --x 100 --y 100

# Window management
et window_list
et window_activate --title "Safari"

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
| `window_find` | Find window | `title`, `processId` |
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
- **Platform**: macOS x64 / arm64

## Installation

### Option 1: Direct Download

Choose the correct version based on your Mac chip architecture:

**Apple Silicon (M1/M2/M3/M4):**
```bash
sudo cp et-arm64 /usr/local/bin/et
sudo chmod +x /usr/local/bin/et
```

**Intel Mac:**
```bash
sudo cp et-x64 /usr/local/bin/et
sudo chmod +x /usr/local/bin/et
```

**How to detect chip architecture:**
```bash
uname -m
# arm64 = Apple Silicon
# x86_64 = Intel
```

### Option 2: Homebrew Installation (recommended)

```bash
# Add tap (coming soon)
brew tap maomiaent/easytouch
brew install easytouch
```

### Option 3: Build from Source

```bash
# Clone the repository
cd tools/EasyTouch/EasyTouch-Mac

# Intel Mac (x64)
dotnet publish -c Release -r osx-x64 --self-contained true -p:PublishAot=true
# Output: bin/Release/net10.0/osx-x64/publish/et

# Apple Silicon (arm64)
dotnet publish -c Release -r osx-arm64 --self-contained true -p:PublishAot=true
# Output: bin/Release/net10.0/osx-arm64/publish/et

# Build universal binary (optional, requires binaries for both architectures)
lipo -create bin/Release/net10.0/osx-x64/publish/et \
             bin/Release/net10.0/osx-arm64/publish/et \
             -output et-universal
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

## Permissions

### Accessibility Permission

When using mouse/keyboard control features for the first time, the system will prompt you to grant Accessibility permission:

1. Open **System Settings** → **Privacy & Security** → **Accessibility**
2. Click the **+** button
3. Select **Terminal** or the app you are using
4. Enable the permission toggle

### Screen Recording Permission

If you use the screenshot feature, you need to grant Screen Recording permission:

1. Open **System Settings** → **Privacy & Security** → **Screen Recording**
2. Add **Terminal** or the app you are using

## Notes

1. **Accessibility Permission**: Mouse and keyboard control require Accessibility permission; please grant it in System Settings
2. **Security**: macOS security mechanisms may block some operations; use only in trusted environments
3. **Chip Architecture**: 
   - Apple Silicon (M1/M2/M3/M4) users should use `et-arm64` for best performance
   - Intel Mac users should use `et-x64`
   - The arm64 version can also run the x64 version via Rosetta 2 on Apple Silicon, but with reduced performance
4. **Sandbox**: Some features may be restricted when running in a sandboxed environment

### Chip Architecture Compatibility

| Your Mac | Recommended Version | Compatible Versions |
|---------|---------|---------|
| M4, M3, M2, M1 | et-arm64 | et-arm64, et-x64 (Rosetta 2) |
| Intel Core i/i5/i7/i9 | et-x64 | et-x64 |

**Detect architecture:**
```bash
# Run in Terminal
uname -m
# Output arm64 = Apple Silicon
# Output x86_64 = Intel
```

## Troubleshooting

### Command Not Responding
- Check Accessibility permission: System Settings → Privacy & Security → Accessibility
- Restart the Terminal app to apply permission changes

### Screenshot Fails
- Check Screen Recording permission: System Settings → Privacy & Security → Screen Recording
- Ensure the output directory is writable

### Permission Errors
```bash
# View detailed error information
et mouse_position 2>&1

# Reset permission attempt
sudo tccutil reset All
```

## Comparison with Other Automation Tools

| Feature | EasyTouch | AppleScript | Automator | Shortcuts |
|------|-----------|-------------|-----------|-----------|
| CLI Support | ✅ | ⚠️ | ❌ | ❌ |
| MCP Integration | ✅ | ❌ | ❌ | ❌ |
| Cross-platform | ✅ (W/L/M) | ❌ | ❌ | ❌ |
| Learning Curve | Low | High | Medium | Low |
| Performance | High | Medium | Medium | Medium |
