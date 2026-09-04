# System Architecture

Short notes on the Linux boot process and init systems.

## Boot sequence

```text
BIOS or UEFI -> bootloader -> kernel and initramfs -> init system (PID 1)
```

- **BIOS/UEFI** performs the initial hardware setup and selects a boot device.
- **GRUB** can present a menu and load the selected kernel and initramfs.
- **The kernel** detects hardware and mounts the initial root environment.
- **The init system** starts userspace services. On most current distributions this is systemd.

The EFI System Partition is normally FAT-formatted and often mounted at `/boot/efi`. It is different from the Linux root filesystem.

## GRUB files

On Debian and Ubuntu, `/etc/default/grub` contains settings that can be edited. `/boot/grub/grub.cfg` is generated and should not normally be edited by hand.

```bash
sudo update-grub
```

On distributions that use the generic GRUB command, the output path must match that distribution:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

**Common mistake:** changing `/etc/default/grub` but forgetting to regenerate the GRUB configuration.

## Inspecting boot information

```bash
lsblk -f
findmnt /boot
dmesg --level=err,warn
journalctl -b
```

- `lsblk -f` shows block devices, filesystems and mount points.
- `findmnt /boot` checks whether `/boot` is a separate mount.
- `dmesg` shows kernel messages; access may require additional permissions.
- `journalctl -b` limits the journal to the current boot.

## systemd basics

`systemd` runs as PID 1 on a systemd-based distribution. A unit can be active now without being enabled for the next boot, and the reverse is also possible.

```bash
systemctl status ssh
systemctl is-active ssh
systemctl is-enabled ssh
systemctl get-default
systemctl list-units --type=service
systemctl list-unit-files --type=service
```

`list-units` shows units currently loaded in memory. `list-unit-files` shows installed unit files and their enablement state.

**Common mistake:** treating `enabled` as if it meant `running`. Use both `is-enabled` and `is-active` when that distinction matters.
