# RSDK Android Builds

GitHub Actions workflows for building Retro Engine SDK decompilations as native Android APKs.

## Engines

| Workflow | Engine | Game | Status |
|----------|--------|------|--------|
| `build-rsdkv1-android.yml` | RSDKv1 | Retro-Sonic (2006-2007) | 🔧 |
| `build-rsdkv2-android.yml` | RSDKv2 | Sonic Nexus (2008) | 🔧 |

## Related Projects

- **[touchHLE-Web](https://github.com/Greenzin1/touchHLE-Web)** — Fork of touchHLE (high-level emulator for iPhone OS apps) with pure Rust ARM interpreter for WASM/web builds

## How It Works

Each workflow:
1. Clones the decompilation repo
2. Creates `Android.cmake` platform file (missing from upstream)
3. Creates `androidHelpers.hpp/cpp` (replaces iOS `cocoaHelpers`)
4. Patches `RetroEngine.hpp` for Android platform detection
5. Builds SDL2 2.30.12 + libogg + libvorbis from source with Android NDK 26
6. Cross-compiles the engine as `libRSDKvX.so` (arm64-v8a)
7. Packages APK with SDL2 Java Activity wrapper

## Game Data

You must supply your own legally obtained game data files:
- **RSDKv1:** `Data.bin` from Retro-Sonic SAGE 2007 demo
- **RSDKv2:** `Data.rsdk` from Sonic Nexus 2008 demo

Place data files in `/sdcard/Android/data/com.rsdkmodding.rsdkvX/files/` on your device.

## Usage

Trigger a build via:
- Push to `main`
- Manual dispatch from the Actions tab

APKs are uploaded as artifacts after each build. Debug-signed.

## Build Dependencies

- **Android NDK 26** — cross-compilation toolchain
- **SDL2** 2.30.12 — audio/input/window management
- **libogg** + **libvorbis** — audio codecs
- **SDL2 Java Activity** — Android activity wrapper

## Credits

- **RSDKModding** — official decompilation repos
- **danielgpinheiro** — RSDKv1 Xbox port reference
- **SDL2, libogg, libvorbis** — compiled from source by GitHub Actions
