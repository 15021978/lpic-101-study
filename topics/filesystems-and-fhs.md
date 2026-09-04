# Filesystems and the FHS

Notes for checking storage, mounts and important Linux directories.

## Start with read-only checks

```bash
lsblk -f
findmnt
df -hT
du -sh /var/log
```

- `lsblk -f` relates block devices to filesystems and mount points.
- `findmnt` shows the current mount tree.
- `df -hT` reports space by mounted filesystem.
- `du -sh` estimates the space used by a directory.

`df` and `du` answer different questions. A large deleted file can still occupy filesystem space while a process keeps it open, so their totals do not always match.

## Mount information

```bash
findmnt /home
cat /etc/fstab
mount
```

`/etc/fstab` describes mounts that should be created automatically. The active mount table must still be checked because a configured mount can fail.

**Common mistake:** editing `/etc/fstab` and rebooting without testing the entry. A safer lab check is:

```bash
sudo mount -a
findmnt
```

## Filesystem checks

`fsck` should normally be used on an unmounted filesystem. Running it on a mounted filesystem can damage data.

```bash
sudo umount /dev/device
sudo fsck /dev/device
```

The device name must be verified first with `lsblk` or `findmnt`; placeholders should never be copied blindly.

## Directories I review

| Directory | Typical purpose |
| --- | --- |
| `/etc` | System-wide configuration |
| `/var` | Variable data such as logs, caches and spools |
| `/home` | User home directories |
| `/boot` | Kernel, initramfs and bootloader files |
| `/usr` | Most installed programs, libraries and shared data |
| `/tmp` | Temporary files |
| `/run` | Runtime state created since boot |
| `/proc` | Process and kernel information exposed as a virtual filesystem |
| `/sys` | Device and kernel information exposed as a virtual filesystem |
