# Third-party notices

The following third-party material is used in the STM32 Debug Board V1.0. These notices supplement, rather than replace, the notices and licence texts supplied by the respective rights holders. The original project licences in [LICENSE.md](LICENSE.md) do not relicense this material.

## KiCad standard libraries

Standard KiCad symbols, footprints and models used by the project originate from the KiCad library community. The libraries use [CC BY-SA 4.0](LICENSES/CC-BY-SA-4.0.txt) with the [KiCad Libraries Exception](https://www.kicad.org/libraries/license/). The exception permits electronic designs using these libraries to be licensed independently. Separately redistributed library collections retain their original terms and attribution.

## SnapMagic Search (formerly SnapEDA)

The following project-local CAD files originate from SnapEDA/SnapMagic Search. The component source links identify the corresponding manufacturer and part; attribution is to SnapMagic Search and the original CAD authors as applicable. Manufacturer names identify the components and do not imply that the manufacturer authored every CAD file.

| File (relative to `hardware/libs/`) | Source / component |
| --- | --- |
| `symbols/FTSH-107-01-L-DV-K-A.kicad_sym` | [Samtec FTSH-107-01-L-DV-K-A](https://www.snapeda.com/parts/FTSH-107-01-L-DV-K-A/Samtec%20Inc./view-part/) |
| `footprints/Project.pretty/SAMTEC_FTSH-107-01-L-DV-K-A.kicad_mod` | Same source |
| `symbols/TS-1187A-B-A-B.kicad_sym` | [XKB TS-1187A-B-A-B](https://www.snapeda.com/parts/TS-1187A-B-A-B/XKB%20Connection/view-part/) |
| `footprints/Project.pretty/SW_TS-1187A-B-A-B.kicad_mod` | Same source |
| `models_3d/TS-1187A-B-A-B--3DModel-STEP-56544.STEP` | Same source |
| `models_3d/TYPE-C-31-M-12--3DModel-STEP-56544.STEP` | [HRO TYPE-C-31-M-12](https://www.snapeda.com/parts/TYPE-C-31-M-12/HRO%20Electronics%20Co.%2C%20Ltd./view-part/) |

The general licence for these standalone design files is [CC BY-SA 4.0](LICENSES/CC-BY-SA-4.0.txt) with SnapMagic Design Exception 1.0, subject to any more specific notices supplied with the individual downloads. The exception allows combined board designs and manufactured boards to use independent licence terms. It does not relicense the standalone CAD assets.

The files have been integrated into the project-local library structure. Any project-specific renaming or edits (including model-path changes) should be documented with the affected files. Further adaptations of standalone files remain subject to their respective terms.

- [SnapMagic licensing FAQ](https://www.snapeda.com/about/FAQ/)
- [Design Exception 1.0](https://support.snapmagic.com/en/articles/2957815-what-is-the-design-exception-1-0)
- [Terms of Service, sections 4 and 5.1(g)](https://www.snapeda.com/about/terms/)

The site terms impose a limit on redistribution of Design Files through one location without prior written permission. This notice does not waive that limit or grant permission beyond the source terms.

## Samtec / CADENAS STEP model

`hardware/libs/models_3d/FTSH-107-01-L-DV-K-A.stp`

- Component supplier: Samtec Inc.; FTSH-107-01-L-DV-K-A.
- CAD data: CADENAS GmbH / PARTsolutions.
- Component reference: [Samtec component page on SnapMagic](https://www.snapeda.com/parts/FTSH-107-01-L-DV-K-A/Samtec%20Inc./view-part/).
- Licence identified in the STEP header: [CC BY-ND 4.0](LICENSES/CC-BY-ND-4.0.txt).
- [CADENAS licence information](https://www.cadenas.de/en/manuals/partwarehouse/?page=section_65_1571206023862.html).

This model is intended to be retained unmodified, with its original header. Redistribution of adapted versions of the model is not granted by this licence. Any separate permission for model-derived artwork must be respected; the project's CC BY 4.0 documentation licence does not extend to this model.

## STM32 firmware

Paths below are relative to `software/stm32_debug_board_test/`.

| Component | Copyright holder(s) | Licence / existing text |
| --- | --- | --- |
| CMSIS Core | Arm Limited | [Apache-2.0](software/stm32_debug_board_test/Drivers/CMSIS/LICENSE.txt) |
| STM32F4 CMSIS Device | Arm Limited and STMicroelectronics | [Apache-2.0](software/stm32_debug_board_test/Drivers/CMSIS/Device/ST/STM32F4xx/LICENSE.txt) |
| STM32F4 HAL | STMicroelectronics | [BSD-3-Clause](software/stm32_debug_board_test/Drivers/STM32F4xx_HAL_Driver/LICENSE.txt) |
| STM32CubeMX-generated initialisation code, startup, linker scripts and project templates | STMicroelectronics and other identified rights holders | Original file notices and applicable ST terms; see [firmware licence](software/stm32_debug_board_test/LICENSE). |

Sources: [STM32CubeF4](https://github.com/STMicroelectronics/STM32CubeF4), [upstream licensing summary](https://github.com/STMicroelectronics/STM32CubeF4/blob/master/LICENSE.md), [STM32CubeMX documentation and licence agreements](https://www.st.com/en/development-tools/stm32cubemx.html). Original copyright notices and supplied licence files must remain intact. No licence is asserted here over the STM32CubeMX or STM32CubeIDE tools themselves.

## Redistribution Notes

The SnapEDA/SnapMagic CAD files are included under CC BY-SA 4.0 with Design Exception 1.0. The application of the redistribution limit in Section 5.1(g) of the SnapMagic Terms of Service to embedded CAD data and historical Git revisions has not been independently confirmed.

If any copyright holder identifies a licensing concern, please raise an issue in this repository so that it can be addressed.

## STM32CubeMX Code Generation

- STM32CubeMX version: 6.17.0
- Device database: DB.6.0.170
- STM32CubeF4 firmware package: V1.28.3
- ST licence agreement: [SLA0048 — Revision 6, February 2026](LICENSES/SLA0048_STM32CubeMX.pdf)

ST-generated code remains subject to the applicable ST licensing terms and Additional License Terms. The MIT licence applies only to the original application code.
