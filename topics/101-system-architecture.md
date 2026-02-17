# Domain 101 — System Architecture

Notes for LPIC-101 (System Architecture), focusing on boot process and init systems.

---

## Boot Process (Correct Order)

**BIOS/UEFI → GRUB → Kernel → init/systemd**

This order is commonly tested.

### BIOS (legacy firmware)
- Legacy firmware (older systems)
- Typical Linux filesystems used in `/boot`: **ext2/ext3/ext4** (common in many setups)

### UEFI (modern firmware)
- Modern firmware
- Uses the **EFI System Partition (ESP)**, commonly mounted at:
  - `/boot/efi`
- Typical filesystem for ESP: **FAT32 (vfat)**

### GRUB (bootloader)
- Bootloader responsible for loading the Linux kernel.

Important files:
- `/etc/default/grub` → main configuration file you edit
- `/boot/grub/grub.cfg` → auto-generated configuration (**do not edit manually**)

Generate/rebuild `grub.cfg` (depends on distro):
- Debian/Ubuntu:
  - `sudo update-grub`
- Generic approach:
  - `sudo grub-mkconfig -o /boot/grub/grub.cfg`

### Kernel
- The core of the operating system.

---

## Init Systems

### systemd (modern init system)
- The most common modern init system.
- Runs as **PID 1**.

### SysVinit (legacy init system)
- Older init system (conceptual knowledge is useful for the exam).

---

## systemd Basics (Commands)

Start a service now:
```bash
sudo systemctl start <service>

Enable a service at boot:
sudo systemctl enable <service>

Switch to another target immediately:
sudo systemctl isolate <target>

Set the default target:
sudo systemctl set-default <target>

View the current default target:
systemctl get-default

List active units (what is running/loaded now):
systemctl list-units

List unit files installed on the system (what exists on disk):
systemctl list-unit-files

List targets (installed unit files of type target):
systemctl list-unit-files --type=target

Exam tip:
list-units shows what is in use now.
list-unit-files shows what exists on the system.

---

### Units
What are systemd “units”?

Units are not processes.
A unit is a systemd configuration object that defines:
	•	what to start
	•	when to start
	•	how to start
	•	how to stop/manage it

A unit may create one or more processes, but the unit itself is not a process.

Common unit types
	•	service → defines a service (e.g., ssh.service)
	•	target → logical grouping of units (e.g., multi-user.target)
	•	mount → defines a mount point
	•	socket → defines a socket
	•	timer → defines a scheduled task

Correct mental model

unit file (.service/.target) → systemd reads the unit → systemd starts it → process(es) run

“Units are definitions managed by systemd; processes are running instances.”
