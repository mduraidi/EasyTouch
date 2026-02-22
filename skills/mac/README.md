# EasyTouch macOS

## Chip Architecture Guide

The macOS version supports two chip architectures:

### 📦 File Description

- **et-x64** - Intel Mac version (compatible with Apple Silicon via Rosetta 2)
- **et-arm64** - Apple Silicon native version (M1/M2/M3/M4)
- **SKILL.md** - Full usage documentation

### 🔍 How to Choose the Right Version

**Method 1: Use Terminal to detect**
```bash
uname -m
```
- Output `arm64` → use `et-arm64`
- Output `x86_64` → use `et-x64`

**Method 2: Check About This Mac**
1. Click the Apple menu in the upper-left corner → About This Mac
2. Check the chip information
   - Shows "Apple M1/M2/M3/M4" → use `et-arm64`
   - Shows "Intel" → use `et-x64`

### 🚀 Installation

**Apple Silicon (recommended):**
```bash
sudo cp et-arm64 /usr/local/bin/et
sudo chmod +x /usr/local/bin/et
```

**Intel Mac:**
```bash
sudo cp et-x64 /usr/local/bin/et
sudo chmod +x /usr/local/bin/et
```

### ⚡ Performance Tips

- **Apple Silicon users**: Strongly recommended to use `et-arm64` for better performance and lower power consumption
- **Universal binary**: If you need to support both architectures, you can use `lipo` to create a universal binary (file size will increase)

### 🔄 Rosetta 2 Compatibility

Apple Silicon Macs can run `et-x64` via Rosetta 2, but with slight performance loss. If you are unsure of your chip type, `et-x64` is the safest choice (best compatibility).

### 📋 Troubleshooting

**"Cannot be opened because the developer cannot be verified"**
```bash
# Solution 1: Right-click -> Open
# Solution 2: Remove quarantine attribute
xattr -d com.apple.quarantine /usr/local/bin/et
```

**"Architecture mismatch" error**
```bash
# This means you downloaded the wrong architecture version
# Please re-download the version matching your architecture
```

### 🔗 Related Links

- Full documentation: SKILL.md
- Project homepage: https://github.com/maomiaent/easytouch
- Issue reporting: https://github.com/maomiaent/easytouch/issues
