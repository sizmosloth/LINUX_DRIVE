# Project Log: Replacing Windows with Fedora (Dual Boot with Mint)

Purpose:
*A record of how I removed Windows 10 and installed Fedora as a second OS alongside Linux Mint, without losing Mint or my data — and also a record of how many times my heart nearly stopped working while doing it.*

Mood while writing this: relieved, slightly traumatized, weirdly PROUD.

---

## My Starting Setup

Laptop: Dell Inspiron 3558
CPU: Intel i3 5th gen
RAM: 4 GB
Disk: 256 GB SSD

Before:
```
EFI        100 MB
Windows    125 GB
Common     67 GB   (NTFS, shared data)
Mint       60 GB
```

Goal:
```
EFI        100 MB
Fedora     ~111 GB
Common     ~63 GB   (untouched)
Mint       ~63 GB   (untouched)
```

---

## Why I Did This

I wanted to remove Windows completely since I wasn't using it, and add Fedora as a second Linux OS to learn on, without breaking my existing Mint setup or losing any files.

Translation: I had 125 GB doing absolutely nothing except occasionally reminding me Windows existed. So i said "get lost windows".

---

## Tools I Used

- **GParted** — to delete Windows partitions and create new ones for Fedora
- **GNOME Disks** — to flash the Fedora ISO onto a USB pendrive
- **sha256sum** — to verify the ISO download wasn't corrupted
- **efibootmgr** — to clean up the boot menu afterward

---

## Step-by-Step: What I Actually Did

### 1. Backed up important files
Copied Documents, Pictures, and anything important to a pendrive before touching any partitions.

### 2. Downloaded Fedora Workstation and made a Live USB
```bash
sudo apt install gnome-disk-utility -y
```
Used the **Disks** app → Restore Disk Image → selected the Fedora `.iso`.

### 3. Verified the ISO wasn't corrupted
```bash
sha256sum ~/Downloads/Fedora-Workstation-Live-44-1.7.x86_64.iso
```
Compared the output against Fedora's official CHECKSUM file:
```bash
wget https://download.fedoraproject.org/pub/fedora/linux/releases/44/Workstation/x86_64/iso/Fedora-Workstation-44-1.7-x86_64-CHECKSUM
sha256sum -c Fedora-Workstation-44-1.7-x86_64-CHECKSUM --ignore-missing
```
Result: matched exactly. ISO was fine.

### 4. Booted into Mint and opened GParted
Checked the existing partition table before touching anything:
```bash
sudo parted -l
sudo fdisk -l
```
Confirmed: GPT partition table, EFI partition, Windows (NTFS), Mint (ext4).

This was the "look both ways before crossing" step. Stared at the output like it was a rmbk reactor and without AZ 5 button.

### 5. Deleted Windows partitions in GParted
Right-clicked the Windows NTFS partition and any recovery partitions → Delete → Apply.
This freed up the space as unallocated.

Clicked Apply and my hand genuinely hovered over the screenshot button in case I needed evidence for the insurance claim against my own decisions. Nothing exploded. 125 GB just... became free. Felt happy because windows gone bcz who tf uses windows!.

### 6. Manually created Fedora's partitions (instead of letting the installer guess)
Fedora 44's new installer kept misreading the unallocated space, so instead of trusting its automatic partitioning, I created the partitions myself in GParted first:

| Size | Filesystem | Purpose |
|---|---|---|
| 4 GB | linux-swap | swap |
| 2 GB | ext4 | `/boot` |
| ~111 GB | btrfs | `/` (root) |

This avoided the installer bug completely — once the partitions already existed, Fedora's "Mount Point Assignment" screen just had to use them, not invent them.

Fedora 44's installer kept hallucinating partitions that didn't exist, like `sda2` and `sda3` appearing out of thin air. Genuinely felt like I was being gaslit by an installer. Solution: stop trusting it, do its job myself.

### 7. Installed Fedora using Mount Point Assignment
Mapped manually:
```
sda1  (EFI, 100MB)   → /boot/efi   (NOT reformatted — reused existing one)
sda3  (ext4, 2GB)    → /boot       (reformatted)
btrfs (~111GB)       → /           (reformatted)
sda2  (swap, 4GB)    → swap
```
Left Mint's and Common's partitions completely unassigned.

### 8. Hit a USB read error mid-install
```
erofs (device loop0): read error -117 ... corrupted compressed data
```
This was the live USB pendrive failing to read itself (likely a flaky 8GB drive), not a hard drive problem. Confirmed by booting back into Mint immediately after — it worked perfectly, proving the hard drive was untouched.

**Fix:** Re-flashed the same ISO onto the pendrive again and retried — it installed successfully the second time.

This is the part where I left to use my phone for five minutes and came back to a wall of red error text scrolling forever like the Matrix had a panic attack. Heart rate: 1000+. Immediately assumed I'd nuked both OSes and possibly the SSD itself. Booted into Mint out of pure desperation expecting a black screen of doom — it opened like nothing happened. 10/10 emotional rollercoaster for a pendrive's fault, not mine.

### 9. First boot — multiple GRUB entries
After install, GRUB showed:
- Fedora (main entry)
- Fedora rescue entry
- Windows Boot Manager (leftover, pointing to nothing since Windows was deleted)
- Many Linux Mint entries (one per installed kernel version — totally normal)

Opened the boot menu expecting "Fedora" and "Mint," got a list that looked like a Linux distro buffet. Briefly considered that I had somehow installed every OS that ever existed. Turns out it was just Mint's kernel history flexing on me. Windows Boot Manager was the real ghost in the machine — dead OS, still haunting my GRUB menu like it pays rent here.

### 10. Cleaned up the boot menu
Checked all boot entries:
```bash
sudo efibootmgr -v
```
Found the dead Windows entry and removed it:
```bash
sudo efibootmgr -b 0000 -B
```
Left Mint's entry (labeled "ubuntu" since Mint is Ubuntu-based) and Fedora's entry alone.

Set Fedora to only keep 1 kernel going forward, so it won't accumulate extra GRUB entries over time:
```bash
sudo dnf config-manager setopt installonly_limit=1 -y
```

To trim Mint's old kernel entries: open Mint's **Update Manager → View → Linux Kernels**, keep the newest 1–2, remove the rest, then run `sudo update-grub` to refresh the GRUB menu.

---

## Things I Learned

- **GPT vs unallocated space isn't the problem when an installer "invents" fake partitions** — sometimes the installer itself has a bug reading free space, and the fix is to just pre-create the partitions yourself in GParted instead of trusting automatic detection.
- **`efibootmgr -v`** lists every EFI boot entry on the disk, including dead ones pointing to deleted OSes.
- A **USB read error during install doesn't mean your hard drive is corrupted** — it's worth booting your existing OS right after a crash to confirm the disk itself is fine before panicking.
- Always **verify ISO downloads with `sha256sum`** before blaming the download when something goes wrong — it rules out one variable immediately.
- GRUB will list **one entry per installed kernel version** automatically — lots of Mint entries isn't corruption, it's kernel history.

---

## Final Result

```
EFI        100 MB   (shared, untouched)
Fedora     ~111 GB  (new)
Common     ~63 GB   (untouched, all data intact)
Mint       ~63 GB   (untouched, all data intact)
```

GRUB now boots cleanly into either Fedora or Mint, with the dead Windows entry removed.

Windows: deleted. Mint: alive and unbothered the whole time, like it had nothing to do with the chaos happening one partition over. Fedora: installed, surprisingly calm now after almost giving me a small heart event. Me: still mentally recovering but also kind of a legend for not panicking into a full reinstall of everything.

---

## My Notes
*Next time I do a Linux install, verify the ISO checksum first, and if the installer's auto-partitioning looks wrong, don't trust it — make the partitions manually in GParted instead.*

*Also: pendrives are not to be trusted. They will betray you at the worst possible moment, specifically the one time you look away to check your phone. Buy a better pendrive. Or befriend one you've bonded with through trauma, like i have used 8 years old sd card and used a new 2 hours old sd card reader*

Status: survived. Would do again. Probably will, because that's just how dual-booting works.
Special thanks to : My Pendrive running on hopes and dreams, Chat gpt and Love to Claude. 