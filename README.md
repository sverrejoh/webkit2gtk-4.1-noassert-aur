# webkit2gtk-4.1-noassert Fork

This is a fork of the official Arch Linux webkit2gtk-4.1 package that builds WebKit without debug assertions.

## Purpose

This fork exists to provide compatibility with applications (like Microsoft Intune) that trigger WebKit assertions which are harmless but cause crashes on Arch Linux due to different build configurations:

- **Arch Linux**: Builds WebKit with assertions enabled
- **Ubuntu/Debian**: Build WebKit with `-DNDEBUG` (assertions disabled)

## Changes from Upstream

1. Package name: `webkit2gtk-4.1-noassert`
2. Install path: `/opt/webkit2gtk-noassert/` (to avoid conflicts)
3. Build flags: Added `-DNDEBUG` to disable assertions
4. Disabled documentation and minibrowser for faster builds