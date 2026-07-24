# RSDK iOS Builds

GitHub Actions workflows for building Retro Engine SDK decompilations as native iOS arm64 binaries.

## Engines

| Workflow | Engine | Game | Status | Upstream Repo |
|----------|--------|------|--------|---------------|
| `build-rsdkv1.yml` | RSDKv1 | Retro-Sonic (2006-2007) | CMake build | [RSDKModding/RSDKv2-Decompilation](https://github.com/RSDKModding/RSDKv2-Decompilation) + RSDKv1 format |
| `build-rsdkv2.yml` | RSDKv2 | Sonic Nexus (2008) | CMake build | [RSDKModding/RSDKv2-Decompilation](https://github.com/RSDKModding/RSDKv2-Decompilation) |
| `build-rsdkv3.yml` | RSDKv3 | Sonic CD (2011) | Xcode project | [RSDKModding/RSDKv3-Decompilation](https://github.com/RSDKModding/RSDKv3-Decompilation) |
| `build-rsdkv4.yml` | RSDKv4 | Sonic 1 & 2 (2013) | Xcode project | [RSDKModding/RSDKv4-Decompilation](https://github.com/RSDKModding/RSDKv4-Decompilation) |
| `build-rsdkv5.yml` | RSDKv5 | Sonic Mania | CMake build | [WamWooWam/RSDKv5-Decompilation](https://github.com/WamWooWam/RSDKv5-Decompilation) |

## Usage

Trigger a build via:
- Push to `main`
- Manual dispatch from the Actions tab

Artifacts (`.ipa` files) are uploaded after each build.

## Game Data

You must supply your own legally obtained game data files:
- **RSDKv1:** `Data.bin` from Retro-Sonic SAGE 2007 demo
- **RSDKv2:** `Data.rsdk` from Sonic Nexus 2008 demo
- **RSDKv3:** `Data.rsdk` from Sonic CD iOS
- **RSDKv4:** `Data.rsdk` from Sonic 1 or Sonic 2 iOS
- **RSDKv5:** `Data.rsdk` from Sonic Mania iOS

The decompilations compile the engine+game code natively. Only the asset data file is needed at runtime.

## Credits / Creditos

### RSDKv1 (Retro-Sonic)

A adaptação RSDKv1 para iOS é baseada no trabalho do **danielgpinheiro** no port de Original Xbox.

The RSDKv1 iOS adaptation is based on **danielgpinheiro**'s work on the Original Xbox port.

- **danielgpinheiro/RSDKv1-xbox**: https://github.com/danielgpinheiro/RSDKv1-xbox
  - Port de Original Xbox usando nxdk (Original Xbox port using nxdk)
  - Adaptação do RSDKv2-Decompilation para ler dados RSDKv1 (adapted RSDKv2-Decompilation to read RSDKv1 data)
  - Tiles 3 bytes com 10-bit tile index (3-byte chunk entries with 10-bit tile index)
  - Fallback GFX para tiles quando GIF não existe (GFX fallback for tiles when GIF is missing)
  - Camera lag estilo CD para scrolling (CD-style camera lag for scrolling)

O RSDKv1 format patch foi adaptado a partir da análise do fork do danielgpinheiro, com referências adicionais de:
The RSDKv1 format patch was adapted from analysis of danielgpinheiro's fork, with additional references from:

- **Rubberduckycooly/RSDK-Reverse**: https://github.com/Rubberduckycooly/RSDK-Reverse
  - Lib de formato de arquivo RSDKv1 (RSDKv1 file format library)
- **Xeeynamo/RSDK**: https://github.com/Xeeynamo/RSDK
  - Reverse engineering dos formatos RSDK e disassemblador de bytecode (RSDK format RE and bytecode disassembler)

### Other Engines

- **RSDKv2-v5**: Built from official decompilation repos by RSDKModding community
- **SDL2, libogg, libvorbis**: Built from source by GitHub Actions workflows

## Build Dependencies

All workflows use `macos-14` (Apple Silicon) runners and build the following from source:
- **SDL2** (2.30.x) — audio/input/window management
- **libogg** + **libvorbis** (+ **libtheora** for v3/v5) — audio/video codecs
- **tinyxml2** (v4/v5 only) — config file parsing

## Patches

The `patches/` directory contains patches for each engine:

| Patch | Description |
|-------|-------------|
| `rsdkv1-format.patch` | RSDKv1 data format support (3-byte chunks, GFX fallback, camera lag) |
| `rsdkv1-ios.patch` | iOS platform support for RSDKv1 |
| `rsdkv2-ios.patch` | iOS platform support for RSDKv2 |
| `rsdkv3-ios.patch` | iOS platform support for RSDKv3 |
| `rsdkv4-ios.patch` | iOS platform support for RSDKv4 |

iOS patches add `RETRO_PLATFORM == RETRO_iOS` code paths that map to existing macOS/Android paths with mobile-specific adjustments (Documents directory, RGBA video, etc.).
