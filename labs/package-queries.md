# Practice Lab: Package Queries

This is a read-only practice exercise. It does not install, remove or upgrade packages.

## Objective

Identify whether a command is installed, which package supplied it and which files belong to that package.

## Debian or Ubuntu

### 1. Locate the command

```bash
command -v ssh
```

This returns the executable selected by the current shell.

### 2. Find the owning package

```bash
dpkg -S /usr/bin/ssh
```

Use the actual path returned in the first step if it is different.

### 3. Inspect the package

```bash
dpkg-query -W openssh-client
apt-cache policy openssh-client
```

The first command checks the local package database. The second also shows repository candidates.

### 4. List installed files

```bash
dpkg -L openssh-client | less
```

## Verification

The executable path should appear in the file list, and the installed version should match the installed line from `apt-cache policy`.

## What I am practicing

- checking before changing a package;
- distinguishing a command path from its package name;
- using the local package database and repository metadata for different questions.
