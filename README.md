# emb-platform

Bazel module `emb_platform`: per-MCU platform code shared by all `prj-*` projects. Depends on [`emb-toolchain`](https://github.com/t0biasm/emb-toolchain).

| Path | Content |
|---|---|
| `avr8/` | ATmega32U4 device description, MCAL tests, flashing notes |
| `c2000/` | TMS320F28P65x device, MCAL (`mcal/f28p65`), FreeRTOS port (`port/freertos`), platform types |
| `c2000/freertos/` | Bazel targets for the FreeRTOS kernel; sources come from the pinned submodule `c2000/freertos/upstream` ([c2000ware-FreeRTOS](https://github.com/TexasInstruments/c2000ware-FreeRTOS), tag `REL_C2000Ware_v5.05.00.00`) |

Vendor documents (datasheets, manuals) are intentionally not stored here; the READMEs link to the vendors.

## FreeRTOS configuration
`FreeRTOSConfig.h` is project specific. A project provides it as a `cc_library` and selects it in its `.bazelrc`:
```
build --@emb_platform//c2000/freertos:config=//applications/<project>:freertos_config
```

## Use in a project
Add this repository and `emb-toolchain` as submodules and override both modules:
```
bazel_dep(name = "emb_toolchain", version = "0.1.0")
bazel_dep(name = "emb_platform", version = "0.1.0")
local_path_override(module_name = "emb_toolchain", path = "toolchain/emb-toolchain")
local_path_override(module_name = "emb_platform", path = "platform/emb-platform")
```
Clone with `git clone --recurse-submodules` so the nested FreeRTOS submodule is fetched.

## Standalone development
`MODULE.bazel` pins `emb-toolchain` by commit (`git_override`, effective only when this repo is the root module). Set the Artifactory URL for the compilers in `user.bazelrc` (see emb-toolchain README).
The unit tests under `*/test` need the `gcc` config from emb-toolchain.
