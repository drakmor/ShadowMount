# ShadowMount (PS5)
**v1.4 by VoidWhisper (+exFAT ffpkg)**

Thanks for supporting ffpkg @Gezine, @earthonion.

**ShadowMount** is a fully automated, background "Auto-Mounter" payload for Jailbroken PlayStation 5 consoles. It streamlines the game mounting process by eliminating the need for manual configuration or external tools (such as DumpRunner or Itemzflow). ShadowMount automatically detects, mounts, and installs game dumps from both **internal and external storage**.

**Compatibility:** Supports all Jailbroken PS5 firmwares running **Kstuff v1.6.7**.

## How to create an exFAT image

**Ubuntu/Debian:**
* sudo apt-get install -y exfatprogs exfat-fuse fuse3 rsync
* truncate -s <image_size> test.ffpkg
* mkfs.exfat -n TESTFFPKG test.ffpkg
* mkdir /mnt/exfat
* mount -t exfat-fuse -o loop test.ffpkg /mnt/exfat
* rsync -r --info=progress2 APPXXXX/ /mnt/exfat/
* umount /mnt/exfat

**Windows:**
Use ImDisk Toolkit

**Why is using USF2 images a bad option?**
Because UFS2 is a case-sensitive file system, and PKG is not, games and programs rely on using a case-insensitive file system, and because of this, many do not work on a built-in drive with a case-sensitive file system (BFS).

---

## 🚀 Key Features

* **Zero-Touch Automation:** No menus, no clicks. ShadowMount scans your storage and installs games automatically.
* **No Extra Apps Required:** Replaces the need for Itemzflow, webMAN, DumpRunner, DumpInstaller, WebSrv, or the Homebrew Store for mounting operations.
* **Automated Asset Management:** Automatically handles assets eliminating the need to copy files through other tools.
* **Hot-Swap Support:** Seamlessly handles unplugging and replugging drives without system instability.
* **Batch Processing:** Capable of scanning and mounting dozens of games simultaneously in seconds.
* **Smart Detection:** Intelligently detects previously mounted games on boot and skips them to ensure zero-overhead startup.
* **Visual Feedback:**
    * **System Notifications:** Non-intrusive status updates for new installations.
    * **Rich Toasts (Optional):** Graphical pop-ups confirming "Game Installed".
      * *Note: This feature requires `notify.elf`. It is kept as a separate payload for users who prefer a cleaner experience without pop-ups.*

---

## 📂 Supported Paths
ShadowMount performs a recursive scan of **Internal Storage** and **All USB Ports** for the following directory structures:

* `/data/homebrew`
* `/data/etaHEN/games`
* `/mnt/usb0/homebrew` through `/mnt/usb7/homebrew`
* `/mnt/usb0/etaHEN/games` through `/mnt/usb7/etaHEN/games`
* `/mnt/ext0/homebrew` & `/mnt/ext0/etaHEN/games`
* `/mnt/ext1/homebrew` & `/mnt/ext1/etaHEN/games`

---

## 🛠️ Installation & Usage

**Prerequisites:**
* Jailbroken PS5 Console.
* **Kstuff v1.6.7**
* *Note: etaHEN and Itemzflow are OPTIONAL. ShadowMount works independently.*

### Method 1: Manual Payload Injection (Port 9021)
Use a payload sender (such as NetCat GUI or a web-based loader) to send the files to **Port 9021**.

1.  Send `notify.elf` (Optional).
    * *Only send this if you want graphical pop-ups. Skip if you prefer standard notifications.*
2.  Send `shadowmount.elf`.
3.  Wait for the notification: *"ShadowMount v1.2 Beta by VoidWhisper Loaded"*.

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
* **Special Thanks:**
    * EchoStretch
    * LightningMods
    * john-tornblom
    * PS5 R&D Community

---

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/voidwhisper)
