# North Star Plan: xDrip+ Wearable Collector Stability

**Version**: v1.0.0
**Last Updated**: 2026-04-02

## 1. Goal
Stabilize the "Force Wear" standalone collector mode in xDrip+ by radically improving Bluetooth BLE reliability and data synchronization hand-offs between the WearOS watch and the Android phone. Ensure modern devices, specifically the TicWatch 5 (WearOS 3+), can operate as a reliable primary CGM receiver without burning battery or hanging in zombie states. 

## 2. Architecture & Constraints
- **Core Stack**: Native Android / WearOS Java (Java 8 compatibility required). 
- **Connectivity**: Bluetooth Low Energy (BLE) GATT for sensor comms. Google Play Services Wearable `MessageApi` (DataMap/Paths) for watch-to-phone inter-process communication (IPC).
- **Constraints**: 
  - Code must remain compatible with legacy Android devices (API >= 21). 
  - Android Gradle Plugin (AGP) and JDK compatibility barriers exist in this specific legacy codebase; rely primarily on `ProdDebug` variants for testing.
  - Do not alter the core Ob1 connection state machine timing without extreme caution; it is highly sensitive to Dexcom's 5-minute advertising window.

## 3. Phased Roadmap
*We follow a strict Crawl, Walk, Run approach.*

- **Phase 1: Missed-Reading Fallback (Complete)**
  - Re-establish a functional watch-to-phone Wearable message path to yield control back to the phone after persistent missed readings.
- **Phase 2: WearOS 3+ Jitter Fixes (Complete)**
  - Mitigate Doze-induced wake-up jitter on WearOS 3+ devices by dynamically engaging the `buggy_samsung` workaround.
- **Phase 3: Transmitter MAC Pre-loading (Complete)**
  - Seed the watch with the transmitter MAC address during sync to bypass 60-second BLE discovery phases.
- **Phase 4: Permission Check Hardening (Complete)**
  - Validate `ACCESS_FINE_LOCATION` robustly before launching the BLE collector.
- **Phase 5: Reliability Testing & Release (Active)**
  - Local compilation, side-loading onto the TicWatch 5, real-world monitoring, and upstream Pull Request preparation.

## 4. Failure Modes / Safety
- **Watch CPU Dozes/Misses Alarm**: The `fail-over` backup alarm fires safely at 7 minutes, closing the connection cleanly.
- **Persistent BLE Disconnects**: If over 30 minutes pass without a reading, the watch automatically messages the phone to take over the collector duties (if user preference allows). 
- **Wear API Disconnects**: Fails safely; settings sync simply caches locally until reconnected.
