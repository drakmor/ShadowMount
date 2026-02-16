# ShadowMountPlus (PS5)


Thanks for ffpkg support: @Gezine, @earthonion and @VoidWhisper for ShadowMount


**ShadowMountPlus** is a fully automated, background "Auto-Mounter" payload for Jailbroken PlayStation 5 consoles. It streamlines the game mounting process by eliminating the need for manual configuration or external tools (such as DumpRunner or Itemzflow). ShadowMount automatically detects, mounts, and installs game dumps from both **internal and external storage**.

**Compatibility:** Supports all Jailbroken PS5 firmwares running **Kstuff v1.6.7**.


## Current image support

`PFS support is experimental.`

| Extension | Mounted FS | Attach backend | Status |
| --- | --- | --- | --- |
| `.exfat` | `exfatfs` | `/dev/lvdctl` | Recommended |
| `.ffpkg` | `ufs` | `/dev/lvdctl` | Test |
| `.ffpfs` | `pfs` | `/dev/lvdctl` | Experimental |

Notes:
- `.exfat` can be switched to MD backend by changing `EXFAT_ATTACH_USE_MDCTL` 

## Mount point naming

Image mountpoints are created under:

`/data/ufsmnt/<image_name>-<fs_suffix>`

## Scan paths

ShadowMount scans these locations:
- `/data/homebrew`
- `/data/etaHEN/games`
- `/mnt/ext0/etaHEN/homebrew`
- `/mnt/ext0/etaHEN/games`
- `/mnt/ext1/etaHEN/homebrew`
- `/mnt/ext1/etaHEN/games`
- `/mnt/usb0/homebrew` .. `/mnt/usb7/homebrew`
- `/mnt/usb0/etaHEN/games` .. `/mnt/usb7/etaHEN/games`
- `/mnt/usb0` .. `/mnt/usb7`
- `/mnt/ext0`
- `/mnt/ext1`
- `/data/ufsmnt` (mounted image content scan)


## Creating an exFAT image

Linux (Ubuntu/Debian):
- `sudo apt-get install -y exfatprogs exfat-fuse fuse3 rsync`
- `truncate -s <image_size> test.exfat`
- `mkfs.exfat -c 32768 test.exfat`
- `mkdir -p /mnt/exfat`
- `mount -t exfat-fuse -o loop test.exfat /mnt/exfat`
- `rsync -r --info=progress2 APPXXXX/ /mnt/exfat/`
- `umount /mnt/exfat`

Windows:
- Recommended: use `make_image.bat` (wrapper for `New-OsfExfatImage.ps1` + OSFMount).
- Requirements:
  - Install OSFMount: https://www.osforensics.com/tools/mount-disk-images.html (must provide `osfmount.com`).
  - Keep `make_image.bat` and `New-OsfExfatImage.ps1` in the same folder.
  - Run `cmd.exe` as Administrator.
- Usage:
  - `make_image.bat "C:\images\game.exfat" "C:\payload\APPXXXX"`
- Behavior:
  - Auto-sizes the image to fit source content.
  - Formats and copies the source folder into the image.
  - Overwrites existing image file (uses `-ForceOverwrite`).
- Optional (fixed size): run PowerShell script directly:
  - `powershell.exe -ExecutionPolicy Bypass -File .\New-OsfExfatImage.ps1 -ImagePath "C:\images\game.exfat" -SourceDir "C:\payload\APPXXXX" -Size 8G -ForceOverwrite`

## Why UFS can be problematic

UFS is case-sensitive, while many PKG/game paths expect case-insensitive behavior. Because of that, some titles may fail or behave incorrectly when mounted from UFS images. This is one reason why UFS support is marked experimental.

## Installation and usage


### Method 1: Manual Payload Injection (Port 9021)
Use a payload sender (such as NetCat GUI or a web-based loader) to send the files to **Port 9021**.

1.  Send `notify.elf` (Optional).
    * *Only send this if you want graphical pop-ups. Skip if you prefer standard notifications.*
2.  Send `shadowmount.elf`.
3.  Wait for the notification: *"ShadowMount"*.

### Method 2: PLK Autoloader (Recommended)
Add ShadowMount to your `autoload.txt` for **plk-autoloader** to ensure it starts automatically on every boot.

**Sample Configuration:**
```ini
!1000
kstuff.elf
!1000
notify.elf  ; Optional - Remove this line if you do not want Rich Toasts
!1000
shadowmount.elf
```

---

## ⚠️ Notes
* **First Run:** If you have a large library, the initial scan may take a few seconds to register all titles.
* **Large Games:** For massive games (100GB+), allow a few extra seconds for the system to verify file integrity before the "Installed" notification appears.

## Credits
* **VoidWhisper** - Lead Developer & Logic Implementation
* **Drakmor** - Evolution
* **Special Thanks:**
    * EchoStretch
    * LightningMods
    * john-tornblom
    * PS5 R&D Community

---

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/voidwhisper)
