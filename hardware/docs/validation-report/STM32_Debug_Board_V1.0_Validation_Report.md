# STM32 Debug Board V1.0

**Hardware and firmware validation report**  
**Report revision:** 1.0 · **Date:** 12 November 2026  
**Result:** PASS — planned bench validation completed

## 1. Executive summary

The first revision of the custom STM32 Debug Board was successfully brought up and tested using its three debug connectors. The STM32F446ZET6 operated from its internal 16 MHz oscillator, the external 8 MHz crystal and the external-crystal-derived 180 MHz PLL configuration. SWD, JTAG, SWO trace, virtual COM port communication, user LEDs, user button, hardware reset and standalone operation were demonstrated.

No PCB design defects were identified during the planned tests, and no corrective hardware rework was required. Three practical issues were encountered and resolved or characterised: an incorrect debugger ground connection, a third-party probe's tool compatibility, and a CubeMX PLL divider setting. None required a board revision.

| Outcome | Result |
|---|---|
| Planned board validation | PASS |
| SWD / JTAG / SWO connector coverage | 3 / 2 / 3 |
| Maximum tested CPU clock | 180 MHz |
| Final standalone USB input current | Approximately 60 mA |
| Hardware defects identified / corrective rework | 0 / none |

This is a functional prototype validation report, not a production qualification or exhaustive MCU peripheral test. Results are based on the project owner's bench observations and debugger checks; the report does not establish that every manufactured board has been tested.

## 2. Hardware and test environment

| Item | Description |
|---|---|
| Target MCU | STM32F446ZET6, LQFP144, Cortex-M4 with FPU, 512 KB flash |
| PCB | V1.0; 75 × 75 mm; two layers; ENIG |
| Manufacture | JLCPCB; five boards ordered, two top-side SMD assembled and three bare; order paid 31 October 2026 |
| Power | J1 USB-C, 5 V nominal; F1 500 mA fuse; U2 AMS1117-3.3 regulator; D1 3.3 V power indicator |
| Oscillator | Y1 8 MHz crystal, PH0/PH1, C15/C16 30 pF |
| Controls | D2/PC13 and D3/PC14 active-low LEDs; S1/PC15 active-low user button; S2/NRST hardware reset |
| Debug ports | J2 ARM 20-pin (2.54 mm); J3 STDC14 (1.27 mm); J4 six-pin SWD (2.54 mm) |
| Software | STM32CubeMX, STM32CubeIDE, STM32CubeProgrammer; STM32CubeF4 V1.28.3; serial terminal |
| Probes | ST-LINK/V2-ISOL (V2J47S7); STLINK-V3MINIE (firmware upgraded); third-party blue 10-pin ST-LINK/V2-style probe (V2J48S7) |
| Instruments | Digital multimeter; USB inline current meter |

## 3. Functional test results

PASS denotes an observed successful test; it does not imply qualification over all voltage, temperature or timing conditions.

| ID | Test | Observed evidence | Result |
|---|---|---|---|
| P01 | Power-up | D1 illuminated; no debugger required | PASS |
| P02 | 3.3 V rail | DMM approximately 3.29 V at regulator and debug reference pins | PASS |
| M01 | Device identification | STM32F446xx, device ID 0x421, Rev A; 512 KB flash reported | PASS |
| M02 | Flash and execution | Test firmware downloaded, executed and verified with genuine ST probes | PASS |
| I01 | User LEDs | D2/D3 alternate at 500 ms intervals | PASS |
| I02 | User button | Press/release messages; 20 ms firmware debounce | PASS |
| I03 | Hardware reset | S2 restarts application; UART startup message observed with probe attached | PASS |
| U01 | UART transmit | Startup message and 1 s heartbeat through J3 VCP | PASS |
| U02 | UART receive | Individual received characters echoed at 115200 baud, 8N1 | PASS |
| D01 | SWD | Programming and breakpoint debugging on J2, J3 and J4 | PASS |
| D02 | JTAG | Programming/debugging on J2 and J3 | PASS |
| D03 | SWO | ITM stimulus port 0 messages on J2, J3 and J4 | PASS |
| C01 | HSI | 16 MHz direct clock, application operating | PASS |
| C02 | HSE | 8 MHz direct clock, oscillator-ready and source status verified | PASS |
| C03 | PLL | 180 MHz, PLL/OverDrive/flash register checks verified | PASS |
| C04 | SWV clock changes | Trace received at 8 MHz and 180 MHz on J3 | PASS |
| R01 | Final application | LED, UART, button and reset regression at 180 MHz | PASS |
| R02 | Standalone | Repeated cold starts and reset at 180 MHz, USB-C only | PASS |

### Power consumption

| Condition | Nominal input | Observed current | Estimated input power |
|---|---:|---:|---:|
| Initial bring-up, HSI baseline | 5 V | ~20 mA | ~100 mW |
| Final 180 MHz firmware, standalone | 5 V | ~60 mA | ~300 mW |

Power is calculated using nominal 5 V, not a simultaneous precision voltage measurement. These are board-level readings under different firmware/test conditions, not a controlled frequency-versus-current experiment. No transient, peak-load or thermal measurements were recorded.

## 4. Debug interface compatibility

| Target connector and probe | SWD / flash / breakpoints | JTAG | SWO | Debugger reset | VCP |
|---|---|---|---|---|---|
| J2 — ST-LINK/V2-ISOL, ribbon | PASS | PASS | PASS | PASS | N/A |
| J3 — STLINK-V3MINIE, ribbon | PASS | PASS | PASS | PASS | PASS |
| J4 — ST-LINK/V2-ISOL, six wires | PASS | N/A | PASS | PASS | N/A |
| J4 — third-party 10-pin probe, three wires | PASS in CubeIDE | N/A | Not tested | Not tested | N/A |

The third-party probe did not connect successfully using STM32CubeProgrammer; this is a tool/probe compatibility observation, not a failure of J4. JTAG and SWO were exercised separately; STDC14 does not carry NJTRST. Connector tests were performed during incremental firmware development; the final 180 MHz regression used J3 and standalone power, not every probe/clock combination.

### Connection precautions

**Disconnect target USB power and probe power before changing wiring.** Use the target's own USB-C supply, check connector orientation and pin 1, and verify signal direction against the particular probe's manual. J4 pin 1 is the target's 3.3 V rail/reference; do not drive it from a probe power output. Do not connect standard ST-LINK/V2 pin 19 or third-party probe 3.3 V/5 V outputs to the powered target.

| J4 pin | Target function | ST-LINK/V2-ISOL 20-pin header |
|---:|---|---:|
| 1 | 3.3 V reference (VAPP) | 1 |
| 2 | GND | **20** |
| 3 | SWDIO / PA13 | 7 |
| 4 | SWCLK / PA14 | 9 |
| 5 | NRST | 15 |
| 6 | SWO / PB3 | 13 |

The mapping above was tested on the isolated probe. Pin 20 is also ground on the standard ST-LINK/V2. Refer to UM1075, Tables 4 and 5 [1]. For J2 and J3, the matching direct ribbon cable was used.

J3's VCP is target USART3: PD8 TX goes to J3 pin 14 (probe input); PD9 RX goes to J3 pin 13 (probe output). Signal names in UM2910 are from the target perspective [2].

## 5. Clock and firmware validation

| Configuration | HSI baseline | Direct HSE | Final HSE + PLL |
|---|---:|---:|---:|
| SYSCLK / HCLK | 16 MHz | 8 MHz | 180 MHz |
| APB1 | 16 MHz | 8 MHz | 45 MHz |
| APB2 | 16 MHz | 8 MHz | 90 MHz |
| SWV core-clock setting | 16 MHz | 8 MHz | 180 MHz |
| PLL | Disabled | Disabled | Enabled |

Final PLL: HSE 8 MHz, M=4, N=180, P=2, Q=8, R=2. The VCO input is 2 MHz and VCO output 360 MHz. PLLP supplies 180 MHz, PLLQ supplies 45 MHz. AHB /1, APB1 /4, APB2 /2; timer clocks 90 and 180 MHz respectively. Voltage Scale 1, OverDrive and five flash wait states are configured. USB data and SDIO are disabled; the 45 MHz PLLQ clock is not a valid 48 MHz USB clock and is not used for USB.

### Register-level evidence

The following expressions were evaluated in the debugger after `SystemClock_Config()` returned. Expected results were reported as observed. Bit definitions are from RM0390 [4].

| Field | Register address / bits | HSE test | Final PLL test |
|---|---|---:|---:|
| SystemCoreClock | CMSIS variable | 8000000 | 180000000 |
| HSERDY | RCC_CR 0x40023800 [17] | 1 | 1 |
| PLLRDY | RCC_CR 0x40023800 [25] | — | 1 |
| SWS | RCC_CFGR 0x40023808 [3:2] | 1 (HSE) | 2 (PLL) |
| ODRDY | PWR_CSR 0x40007004 [16] | — | 1 |
| ODSWRDY | PWR_CSR 0x40007004 [17] | — | 1 |
| LATENCY | FLASH_ACR 0x40023C00 [3:0] | — | 5 |

The generated source also confirms voltage Scale 1 and the PLL and bus prescaler values. `SystemCoreClock` is a calculated value, not an independently measured frequency; register checks plus functional tests establish correct source selection and successful operation.

### Test application and launch configurations

Firmware uses HAL/SysTick timing; LEDs alternate every 500 ms, UART heartbeat and ITM text are emitted every 1 s, and the user button is polled with 20 ms debounce. UART3 is polling-based at 115200, 8N1, no flow control; it demonstrates individual character echo rather than sustained full-rate reception.

| Launch file (in software/stm32_debug_board_test/) | Purpose |
|---|---|
| `stm32_debug_board_test.launch` | SWD |
| `stm32_debug_board_test_JTAG.launch` | JTAG |
| `stm32_debug_board_test_SWV.launch` | SWD + SWV; final HCLK 180 MHz; SWO limit 1 MHz |

For SWV, start the launch, stop at main, open the SWV ITM Data Console, enable stimulus port 0, start trace and resume. The console displays `SWO: Running...`. Update the SWV core clock when changing firmware clock frequency. Launches use relative executable paths and do not require a specific probe serial number.

## 6. Issues encountered and lessons learned

### 6.1 Isolated ST-LINK ground pin selection

**Symptom:** J4 connections were intermittent or failed when using pin 4 of the ST-LINK/V2-ISOL as ground; debugger voltage indications were inconsistent (approximately 1.85–2.55 V).

**Cause and correction:** The standard and isolated ST-LINK/V2 models have different 20-pin assignments. UM1075 Rev 10, Table 5 [1], specifies GND on 12, 18 and 20 for the isolated probe. Pins 4, 6, 8 and 16 are unconnected; 10 and 14 serve SWIM-related purposes. Moving the ground wire to pin 20 restored reliable communication. The direct J2 ribbon connection also worked.

**Residual observation:** The isolated probe subsequently indicated approximately 2.96–2.97 V, while the board measured 3.29 V by DMM; the V3MINIE indicated about 3.27 V. The reason for the difference was not established. Reliable debug operation was demonstrated; no target rail defect was identified. Do not treat probe readings as a substitute for a direct voltage measurement.

### 6.2 Third-party Amazon ST-LINK/V2-style probe

**Symptom:** The blue 10-pin probe failed to connect in STM32CubeProgrammer, including after its firmware update and attempts using different connection/reset settings and reduced frequency. The exact cause was not established.

**Working configuration:** STM32CubeIDE connected over SWD, downloaded the application and hit breakpoints. Only three wires were connected, with the target powered separately:

| Probe pin (tested unit only) | J4 pin | Signal |
|---:|---:|---|
| 2 | 4 | SWCLK |
| 4 | 3 | SWDIO |
| 6 | 2 | GND |

No power or reset wires were connected. SWO, reset through the clone, and legacy ST-LINK Utility were not verified. Other look-alike probes may have different wiring or firmware. Prefer a genuine ST probe for reproducibility; verify the pinout of any alternative before connection.

### 6.3 CubeMX PLLQ divider locked when unused

**Symptom:** With USB and SDIO disabled, PLLQ was greyed out at /2. The generated code would have set a 180 MHz PLLQ output at a 360 MHz VCO frequency. DS10693 Rev 11, Table 43 [3], lists a maximum PLL48 output of 75 MHz. ST Community confirms the UI behaviour [5]. The configuration was not downloaded in this state.

**Tested workaround:** In CubeMX temporarily enable SDIO in one-bit mode, set PLLQ to /8, disable SDIO again, and verify the divider remains /8. Only then regenerate code. The generated `SystemClock_Config()` and `.ioc` both retained PLLQ=8, yielding 45 MHz, and the source diff showed no additional SDIO initialisation or pin assignments. No hand edits to generated clock code were required.

**Lesson:** Review disabled or greyed-out clock outputs and generated code against the datasheet, even when the peripheral is not used. If USB is added in a future design, provide a valid 48 MHz clock.

## 7. Conclusion and limitations

V1.0 meets the planned functional bench validation objectives. The final firmware starts reliably without a probe and supports the intended debug and diagnostic features. The ninth repository commit captures the tested 180 MHz configuration; the earlier HSI and direct HSE baselines remain in Git history (direct HSE commit `3dc5041`).

The following were outside the recorded test scope: BOOT0/system bootloader operation, independent oscillator accuracy and startup margin, exhaustive flash/RAM testing, long-duration stress testing, thermal/environmental qualification, EMC/ESD, USB data, unused MCU peripherals, maximum debug clock speeds and production yield. No claim is made that all boards, all probe variants or all clock/interface combinations have been validated.

The next project milestones are README and connector documentation, final portability and licensing review, and GitHub publication. The two third-party debugger photographs are intended for the README and are not embedded here.

### References

1. STMicroelectronics, [UM1075 — ST-LINK/V2 and ST-LINK/V2-ISOL user manual](https://www.st.com/resource/en/user_manual/dm00026748.pdf), Rev 10, Tables 4–5.
2. STMicroelectronics, [UM2910 — STLINK-V3MINIE user manual](https://www.st.com/resource/en/user_manual/um2910-stlinkv3minie-debuggerprogrammer-tiny-probe-for-stm32-microcontrollers-stmicroelectronics.pdf), Rev 4, Table 4.
3. STMicroelectronics, [DS10693 — STM32F446xC/E datasheet](https://www.st.com/resource/en/datasheet/stm32f446vc.pdf), Rev 11, Table 43.
4. STMicroelectronics, RM0390 — STM32F446xx reference manual, RCC, PWR and FLASH chapters ([device documentation](https://www.st.com/en/microcontrollers-microprocessors/stm32f446/documentation.html)).
5. ST Community, [Why is the Q factor in Clock Configuration gray in Main PLL?](https://community.st.com/stm32cubemx-mcus-29/questioning-by-a-newcomer-why-is-the-q-factor-in-clock-configuration-gray-in-main-pll-12091), 7 September 2022.
6. Project evidence: V1.0 schematic, STM32CubeMX configuration, generated source, launch configurations, Git diffs and owner-reported bench results through 12 November 2026.
