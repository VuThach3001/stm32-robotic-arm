# Build and Rebuild Instructions (STM32 Robotic Arm)

This document explains how to build and rebuild the firmware in this directory using the existing CMake presets.

## Project Root

Run all commands from:

```bash
cd /home/dave301/Desktop/workspace/stm32-robotic-arm
```

## Prerequisites

Install and verify the following tools:

1. `cmake`
2. `ninja`
3. `arm-none-eabi-gcc` and `arm-none-eabi-g++`

Verification:

```bash
cmake --version
ninja --version
arm-none-eabi-gcc --version
arm-none-eabi-g++ --version
```

## CMake Presets Used in This Repository

Defined in `CMakePresets.json`:

- `Debug`
- `Release`

Behavior:

- Generator: `Ninja`
- Toolchain file: `cmake/gcc-arm-none-eabi.cmake`
- Debug binary directory: `build/Debug`
- Release binary directory: `build/Release`

## Normal Build (Incremental)

Use this for day-to-day development.

### Debug

```bash
cmake --preset Debug
cmake --build --preset Debug
```

### Release

```bash
cmake --preset Release
cmake --build --preset Release
```

## Clean Rebuild (Recommended When in Doubt)

Use this when build artifacts are stale, toolchain options changed, or configuration errors appear.

### Debug Clean Rebuild

```bash
rm -rf build/Debug
cmake --preset Debug
cmake --build --preset Debug
```

One-liner:

```bash
rm -rf build/Debug && cmake --preset Debug && cmake --build --preset Debug
```

### Release Clean Rebuild

```bash
rm -rf build/Release
cmake --preset Release
cmake --build --preset Release
```

One-liner:

```bash
rm -rf build/Release && cmake --preset Release && cmake --build --preset Release
```

## Build Outputs

Expected output artifacts are in:

- `build/Debug/` for Debug
- `build/Release/` for Release

Firmware ELF is typically generated as:

- `build/Debug/temp.elf` (Debug)
- `build/Release/temp.elf` (Release)

## Verify Build Result

Check that the ELF exists:

```bash
ls -lh build/Debug/temp.elf
```

Check binary size:

```bash
arm-none-eabi-size build/Debug/temp.elf
```

## Useful Related Files

- `CMakeLists.txt`
- `CMakePresets.json`
- `cmake/gcc-arm-none-eabi.cmake`
- `cmake/stm32cubemx/CMakeLists.txt`
- `STM32F411XX_FLASH.ld`
- `startup_stm32f411xe.s`

## Common Problems and Fixes

### Problem: `arm-none-eabi-gcc` not found

Cause: ARM toolchain is not installed or not in `PATH`.

Fix:

1. Install ARM GNU toolchain.
2. Ensure binaries are on `PATH`.
3. Re-run configure and build.

### Problem: Configuration/cache mismatch

Cause: CMake cache still reflects old settings.

Fix:

```bash
rm -rf build/Debug
cmake --preset Debug
cmake --build --preset Debug
```

### Problem: Build fails after changing generator/toolchain settings

Cause: Existing build tree was configured with different settings.

Fix: delete build folder and configure again from scratch.

## Quick Command Cheat Sheet

```bash
# Debug incremental
cmake --preset Debug
cmake --build --preset Debug

# Debug clean rebuild
rm -rf build/Debug && cmake --preset Debug && cmake --build --preset Debug

# Release incremental
cmake --preset Release
cmake --build --preset Release

# Release clean rebuild
rm -rf build/Release && cmake --preset Release && cmake --build --preset Release
```
