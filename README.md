# webkit2gtk-4.1-noassert

This is a fork of the official Arch Linux webkit2gtk-4.1 package that builds WebKit without debug assertions.

## Purpose

This fork exists to provide compatibility with applications (like Microsoft Intune) that trigger WebKit assertions which are harmless but cause crashes on Arch Linux due to different build configurations:

- **Arch Linux**: Builds WebKit with assertions enabled
- **Ubuntu/Debian**: Build WebKit with `-DNDEBUG` (assertions disabled)

## Installation

### Important: Import PGP Keys First

Before building this package, you **must** import the PGP keys used to sign the WebKitGTK source tarballs:

```bash
gpg --recv-keys 5AA3BC334FD7E3369E7C77B291C559DBE4C9123B 013A0127AC9C65B34FFA62526C1009B693975393
```

These keys belong to the official WebKitGTK maintainers:
- Adrián Pérez de Castro <aperez@igalia.com>
- Carlos Garcia Campos <cgarcia@igalia.com>

You can verify these keys at: https://www.webkitgtk.org/verifying.html

### Building the Package

After importing the keys:

```bash
git clone https://github.com/yourusername/webkit2gtk-4.1-noassert-aur.git
cd webkit2gtk-4.1-noassert-aur
makepkg -si
```

If you get a PGP signature verification error, make sure you've imported the keys as shown above.

## Changes from Upstream

1. Package name: `webkit2gtk-4.1-noassert`
2. Install path: `/opt/webkit2gtk-noassert/` (to avoid conflicts)
3. Build flags: Added `-DNDEBUG` to disable assertions
4. Disabled documentation and minibrowser for faster builds

## Usage

Applications that need to use this version of webkit2gtk should be configured to use the libraries from `/opt/webkit2gtk-noassert`.

You may need to set environment variables:
```bash
export LD_LIBRARY_PATH=/opt/webkit2gtk-noassert/lib:$LD_LIBRARY_PATH
export PKG_CONFIG_PATH=/opt/webkit2gtk-noassert/lib/pkgconfig:$PKG_CONFIG_PATH
```