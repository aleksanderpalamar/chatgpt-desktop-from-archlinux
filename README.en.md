# ChatGPT for Arch Linux

[Português](README.md) | **English**

This repository repackages the official ChatGPT desktop application,
distributed by OpenAI as a `.deb` file, into a native Arch Linux package
managed by `pacman`.

This project is specifically designed for ChatGPT. It is not a general-purpose
Debian package converter.

During packaging, the application files are preserved, while the scripts that
would configure an APT repository on Debian are not executed on Arch Linux.

## Requirements

- Arch Linux x86_64
- The `base-devel` package, which provides `makepkg` and the required build tools
- The official `chatgpt_amd64.deb` file
- Optionally, `pacman-contrib` to use the `updpkgsums` command

## Prepare the source file

Place the `.deb` file in the root of this repository with the following name:

```text
chatgpt_amd64.deb
```

The file is ignored by Git because it is large and distributed separately by
OpenAI.

## Build the Arch package

Open the repository directory and run:

```bash
makepkg -sf
```

`makepkg` validates the SHA-256 checksum configured in the `PKGBUILD`, checks
the dependencies, and creates a file similar to:

```text
chatgpt-26.814.41957-1-x86_64.pkg.tar.zst
```

The `src/` and `pkg/` directories, as well as the generated package, are local
build artifacts and are included in `.gitignore`.

## Install or upgrade

To install the generated package or upgrade an existing installation:

```bash
sudo pacman -U "$(makepkg --packagelist)"
```

You can also build and install it in a single operation:

```bash
makepkg -sfi
```

After installation, open **ChatGPT** from the application menu or run:

```bash
chatgpt
```

## Upgrade to a new ChatGPT version

This is a local package. Therefore, `sudo pacman -Syu` updates its dependencies
but does not automatically download a new version of ChatGPT.

When OpenAI releases a new version:

1. Replace `chatgpt_amd64.deb` with the new file.
2. Update `pkgver` in the `PKGBUILD` to match the version of the new `.deb`.
3. Update the SHA-256 checksum.
4. Build and install the new package.

With `pacman-contrib` installed:

```bash
updpkgsums
makepkg -sf
sudo pacman -U "$(makepkg --packagelist)"
```

Without `updpkgsums`, calculate the checksum manually:

```bash
sha256sum chatgpt_amd64.deb
```

Copy the displayed value into the `sha256sums` field in the `PKGBUILD` before
running `makepkg`.

`pacman` recognizes the installed package as `chatgpt` and performs the upgrade
while preserving the user's configuration.

## Remove

```bash
sudo pacman -Rns chatgpt
```

## AppArmor

The `/etc/apparmor.d/chatgpt` profile included in the `.deb` file is preserved
in the package, but it only takes effect when AppArmor is installed and enabled
on the system.
