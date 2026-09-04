# Package Management

Commands I use to inspect packages before changing a system.

## Debian and Ubuntu

```bash
apt-cache policy openssh-server
apt show openssh-server
dpkg-query -W openssh-server
dpkg -L openssh-server
dpkg -S /usr/bin/ssh
```

- `apt-cache policy` shows available and installed versions.
- `apt show` displays package metadata.
- `dpkg-query -W` checks the local package database.
- `dpkg -L` lists files installed by a package.
- `dpkg -S` finds which installed package owns a path.

Before installing or upgrading packages:

```bash
sudo apt update
sudo apt install package-name
```

`apt update` refreshes package indexes; it does not upgrade installed packages.

Useful checks after an interrupted package operation:

```bash
sudo dpkg --audit
sudo apt-get check
```

## RPM-based systems

```bash
rpm -q bash
rpm -qi bash
rpm -ql bash
rpm -qf /usr/bin/bash
dnf info bash
```

These queries inspect the installed package database. `dnf` also understands configured repositories and dependencies.

## Shared libraries

```bash
ldd /usr/bin/ssh
ldconfig -p
```

`ldd` shows the shared libraries requested by a program. `ldconfig -p` displays the library cache.

**Common mistake:** using `dpkg -i` or `rpm -i` without considering dependencies. The higher-level package tools normally handle repository dependencies more clearly.
