# ESP32 Micromouse

**Status:** In development (November 2026)  
**Competition target:** Spring 2027

An in-progress autonomous maze robot project using an ESP32-WROOM-32E. The current goal is to characterize the owned sensors and drivetrain, then build navigation firmware in small testable modules. This README separates hardware on hand from components and features that still need validation.

## Current hardware inventory

- ESP32-WROOM-32E development board
- TT gearmotors, wheels, chassis, and encoder discs from an existing smart-car kit
- Gikfun IR emitter/receiver kit advertised as 940 nm
- Separate phototransistors already on hand; exact model and spectral response need identification
- Breadboard, prototyping boards, jumper wires, and handheld oscilloscope

The chassis and wheel dimensions have not yet been confirmed to fit a standard micromouse maze. The encoder sensing modules/interface are also not confirmed, so the encoder discs alone do not mean closed-loop odometry is working.

## Candidate architecture

| Function | Plan | Status |
|---|---|---|
| Main controller | ESP32-WROOM-32E, C/C++ with Arduino framework initially | On hand; firmware setup is next |
| Wall detection | Pulse IR emitters and sample phototransistors with ADC; characterize emitter-off/on readings | Parts on hand; spectral and mechanical fit to verify |
| Motor drive | Dual H-bridge such as DRV8833 | Planned; verify motor stall current before selecting |
| Encoder feedback | Read quadrature encoder signals if the kit's encoder boards are available and safe for ESP32 GPIO | To verify |
| Navigation | 16 × 16 maze map and flood-fill route planning | Planned; simulator-first |
| Motion control | PWM drive, encoder-based speed feedback, and PID tuning | Planned; after motor interface is confirmed |

## Development roadmap

1. Identify the emitter, receiver, and encoder board models; record pinouts and voltage levels.
2. Bring up ESP32 build/serial logging and test each GPIO/ADC input with a safe signal.
3. Calibrate each IR sensor by comparing emitter-off and emitter-on readings at several wall distances.
4. Simulate 16 × 16 maze mapping and flood-fill decisions before integrating movement.
5. Select a motor driver after measuring or verifying motor stall current; test each motor at low PWM.
6. Add encoder counting, wheel-speed control, and repeatable cell-length movement.
7. Integrate wall sensing and navigation; run repeated maze trials and record failures as well as successes.

## Goals (not measured results)

- At least 95% wall-classification accuracy in a documented test set
- At least 90% maze completion across repeated practice runs
- Stable cell-to-cell motion using calibrated sensors and encoder feedback

These are targets only. Add results after testing, including maze layout, sample/run count, failures, path length, and solve time.

## Repository structure

```text
firmware/       ESP32 application and peripheral modules
simulator/      Maze and navigation simulation
hardware/       Pin maps, wiring diagrams, chassis notes, and BOM
docs/           Calibration procedures and experiment logs
```

## Reference project

A supplied STM32F411 micromouse ZIP and schematic were reviewed for architectural context. This repository is an independent ESP32 implementation. The reference snapshot includes sensor acquisition and motor/telemetry setup; flood-fill navigation is to be implemented here. Record the upstream URL and license before reusing any source or schematic material.
