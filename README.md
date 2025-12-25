# HATS Installer Payload

A minimal payload for installing HATS (Homebrew App Tools & Store) packs on Nintendo Switch outside of Horizon OS.

Based on [TegraExplorer](https://github.com/shchmue/TegraExplorer) and [hekate](https://github.com/CTCaer/hekate) by CTCaer, naehrwert, and shchmue.

## Features

- **Three Installation Modes**: Choose how your HATS pack is installed
- **Progress Display**: Visual progress bars and status messages
- **Error Logging**: Detailed logs written to `sd:/hats-install.log`
- **Payload Chaining**: Automatically launch another payload after installation
- **Minimal Footprint**: Small payload size for fast loading

## Installation Modes

| Mode | Description |
|------|-------------|
| `replace` | Only replaces files - no deletion (old files may remain) |
| `default` | Deletes `/atmosphere` only, keeps `/bootloader` and `/switch` |
| `clean` | Deletes `/atmosphere`, `/bootloader`, and `/switch` (fresh install) |

## Building

### Prerequisites

- **devkitARM** - ARM toolchain for Nintendo Switch development
- **BDK** - Blue Development Kit (included in this repo)

### Build Commands

```bash
make clean
make
```

The built payload will be output to `build/hats-installer/hats-installer.bin`.

## Usage

### 1. Prepare Files

Place your HATS pack files on the SD card:

```
sd:/hats-staging/
├── atmosphere/
├── bootloader/
├── switch/
└── manifest.json
```

### 2. Configure (Optional)

Create `sd:/config/hats-tools/config.ini` to set the installation mode:

```ini
[hats]
install_mode=default
```

### 3. Launch

Use hekate or another bootloader to launch `hats-installer.bin`.

### 4. What Happens

1. Payload mounts the SD card
2. Reads configuration (if present)
3. Checks for staging directory
4. Performs cleanup based on install mode
5. Copies files from staging to SD root
6. Cleans up staging directory and HATS version files
7. Launches next payload (if `sd:/payload.bin` exists)

## Configuration

The payload reads configuration from `sd:/config/hats-tools/config.ini`:

```ini
[hats]
install_mode=default    ; Options: replace, default, clean
```

## Project Structure

```
hats-payload/
├── source/           # Main source code
│   ├── main.c        # Entry point and install logic
│   ├── fs.c          # File system operations
│   ├── gfx.c         # Graphics utilities
│   └── libs/         # FatFS library
├── bdk/              # Blue Development Kit
├── config-sample.ini # Configuration example
├── Makefile          # Build configuration
└── build/            # Build output directory
```

## Logs

Installation logs are written to `sd:/hats-install.log` for troubleshooting.

## Integration

This payload is part of the HATS ecosystem:
- Works with HATS-Updater for automated updates
- Handles `manifest.json` for version tracking
- Integrates with the HATS pack distribution system

## License

This project is based on TegraExplorer and hekate. Please refer to those projects for their respective licenses.

## Credits

- **CTCaer** - [hekate](https://github.com/CTCaer/hekate)
- **naehrwert** - Tegra exploration work
- **shchmue** - [TegraExplorer](https://github.com/shchmue/TegraExplorer)
