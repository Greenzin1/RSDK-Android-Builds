# RSDK iOS Builds

GitHub Actions workflows for building Retro Engine SDK decompilations as native iOS arm64 binaries.

## Engines

| Workflow | Engine | Game | Status |
|----------|--------|------|--------|
| `build-rsdkv1.yml` | RSDKv1 | Retro-Sonic (2006-2007) | ✅ |
| `build-rsdkv2.yml` | RSDKv2 | Sonic Nexus (2008) | ✅ |
| `build-rsdkv3.yml` | RSDKv3 | Sonic CD (2011) | 🔧 |
| `build-rsdkv4.yml` | RSDKv4 | Sonic 1 & 2 (2013) | 🔧 |
| `build-rsdkv5.yml` | RSDKv5 | Sonic Mania | 🔧 |
| `build-liveexec32.yml` | RSDKLegacy | Sega Genesis emulators (Sonic 1, Spinball) | 🔧 |

## Fork History

### RSDKv1 (Retro-Sonic)

O RSDKv1 e o formato mais antigo do Retro Engine. O RSDKv1-Decompilation nao existe como repo separado, entao o workflow clona o RSDKv2-Decompilation e adapta pra ler dados RSDKv1 (tiles de 3 bytes com 10-bit tile index, fallback GFX, camera lag estilo CD).

The RSDKv1 is the oldest version of the Retro Engine. There's no standalone RSDKv1 decompilation repo, so the workflow clones RSDKv2-Decompilation and patches it to read RSDKv1 data (3-byte chunk tiles with 10-bit tile index, GFX fallback, CD-style camera lag).

Baseado no trabalho do **danielgpinheiro** no port de Original Xbox: https://github.com/danielgpinheiro/RSDKv1-xbox

### RSDKv2 (Sonic Nexus)

O Sonic Nexus de 2008 foi o primeiro jogo publicado com o Retro Engine. A comunidade RSDKModding fez o decompilation oficial. O port pra iOS e o mais simples porque o RSDKv2 ja tinha suporte nativo a iOS no codigo-fonte original (`RETRO_iOS`). So precisamos criar o `cocoaHelpers.hpp`/`.mm` e o `platforms/iOS.cmake` que faltavam.

Sonic Nexus (2008) was the first game released with the Retro Engine. The RSDKModding community made the official decompilation. The iOS port is the simplest because RSDKv2 already had native iOS support in the original source code (`RETRO_iOS`). We just needed to create the missing `cocoaHelpers.hpp`/`.mm` and `platforms/iOS.cmake`.

### RSDKv3 (Sonic CD)

O Sonic CD de 2011 foi feito com o Retro Engine v3. O decompilation tem um Xcode project pro iOS que referencia o `cocoaHelpers.mm` do macOS (que usa AppKit). O nosso workflow substitui por uma versao que usa Foundation/UIKit, cria o `getDocumentsPath()` que o codigo iOS chama mas nunca foi definido, e adiciona tinyxml2 que o repo nao inclui.

Sonic CD (2011) was made with Retro Engine v3. The decompilation has an Xcode project for iOS that references the macOS `cocoaHelpers.mm` (which uses AppKit). Our workflow replaces it with a Foundation/UIKit version, creates the `getDocumentsPath()` that the iOS code calls but was never defined, and adds tinyxml2 which the repo doesn't include.

### RSDKv4 (Sonic 1 & 2)

O Sonic 1 e Sonic 2 de 2013 foram feitos com o Retro Engine v4. O Xcode project do decompilation tem paths errados pros NativeObjects (espera `RSDKv4/RetroGameLoop.cpp` mas o arquivo ta em `RSDKv4/NativeObjects/RetroGameLoop.cpp`). O workflow cria symlinks e o `NativeObjects.hpp` que ta no repo mas em subdiretorio.

Sonic 1 & 2 (2013) were made with Retro Engine v4. The decompilation's Xcode project has wrong paths for NativeObjects (expects `RSDKv4/RetroGameLoop.cpp` but the file is at `RSDKv4/NativeObjects/RetroGameLoop.cpp`). The workflow creates symlinks and handles the `NativeObjects.hpp` umbrella header.

### RSDKv5 (Sonic Mania)

O Sonic Mania de 2017 e o jogo mais famoso feito com o Retro Engine. O CMakeLists.txt do repo original e so pra Android (usa games-controller, game-activity, etc do Android Jetpack). O WamWooWam fork (https://github.com/WamWooWam/RSDKv5-Decompilation) surgiu em 2024 com suporte a iOS, ficou famoso porque em 2023 nao existia nada pro v5, o app foi pra AltStore e tal. Mas ja ta outdated depois da versao 1.1.1 + U de 2024 do repo oficial do RSDKModding.

O nosso workflow cria um CMakeLists.txt proprio pra iOS que usa SDL2, linka as frameworks do iOS, e usa os source files do engine diretamente.

Sonic Mania (2017) is the most famous game made with the Retro Engine. The original CMakeLists.txt is Android-only (uses games-controller, game-activity, etc from Android Jetpack). The WamWooWam fork (https://github.com/WamWooWam/RSDKv5-Decompilation) appeared in 2024 with iOS support, got popular because nothing existed for v5 in 2023, the app was on AltStore etc. But it's already outdated after the official RSDKModding repo's 1.1.1 + U version from 2024.

### RSDKLegacy (liveexec32)

O RSDKLegacy usa o liveexec32 (baseado no Dynarmic) pra rodar binarios Mach-O armv6/v7 no iOS arm64. O projeto original e o Greenzin1/Liveexec32-build, que usa Dynarmic como tradutor dinamico de ARMv7 pra ARM64, com GuestFrameworks completos (UIKit, Foundation, QuartzCore, OpenGLES, etc).

RSDKLegacy uses liveexec32 (based on Dynarmic) to run Mach-O armv6/v7 binaries on iOS arm64. The original project is Greenzin1/Liveexec32-build, using Dynarmic as the dynamic translator from ARMv7 to ARM64, with complete GuestFrameworks (UIKit, Foundation, QuartzCore, OpenGLES, etc).

Os IPAs de teste sao emuladores da Sega of America: Sonic 1 (2009, armv6) e Sonic Spinball (2012, armv7), que continham CPUs M68000 + Z80 + VDP emulados.

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

- **RSDKv1:** Baseado no port de Original Xbox do **danielgpinheiro** - https://github.com/danielgpinheiro/RSDKv1-xbox
- **RSDKv2-v5:** Feito pelas decompilation repos oficiais da comunidade RSDKModding
- **RSDKv5 (iOS fork):** WamWooWam - https://github.com/WamWooWam/RSDKv5-Decompilation
- **Sega of America:** Criadora dos emuladores Genesis/Mega Drive usados nos IPAs de Sonic 1 (2009) e Sonic Spinball (2012) - esses emuladores continham o engine emulado com CPU M68000 + Z80 + VDP, e serviram de base pro liveexec32
- **SDL2, libogg, libvorbis, libtheora:** Compilados do fonte pelo GitHub Actions
- **danielgpinheiro:** Referencia principal do RSDKv1

## Build Dependencies

All workflows use `macos-14` (Apple Silicon) runners and build the following from source:
- **SDL2** (2.30.x) — audio/input/window management
- **libogg** + **libvorbis** (+ **libtheora** for v3/v5) — audio/video codecs
- **tinyxml2** (v3/v4/v5) — config/XML parsing

## IPA

As IPAs sao unsigned - o Apple Jr assina pra voce. Nao precisa de jailbreak nem JIT.

The IPAs are unsigned - Apple Jr signs them for you. No jailbreak or JIT needed.
