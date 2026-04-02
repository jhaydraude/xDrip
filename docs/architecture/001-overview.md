# Wearable Collector Architecture Overview

## Sub-systems Overview
1. **`Ob1G5CollectionService` (Background Service, Wear/App)**
   - The primary Bluetooth Low Energy (BLE) collection state machine. Uses RxAndroidBle to scan and connect to Dexcom G5/G6 transmitters.
   - Extremely sensitive to the 5-minute transmission window of the sensor. Relies heavily on `JoH.getWakeLock` and `AlarmManager` for precise awakening.
   
2. **`ListenerService` (Wear->Phone IPC, Wear)**
   - Wearable Message API receiver on the Watch.
   - It acts as the gateway for syncing preferences and sending asynchronous commands (e.g., `requestPhoneDisableForceWear()`) to the phone over Wi-Fi/Bluetooth depending on what Android Wear transparently abstracts.

3. **`WatchUpdaterService` (Phone->Wear IPC, App)**
   - Wearable Message API transmitter/receiver on the Phone.
   - Authoritative hub for sending configuration changes. During collection toggle (`sendPrefSettings`), it broadcasts the state (e.g., `transmitter_mac`, `dex_txid`) to the watch's `ListenerService`.
   - Listens for fail-over requests on `WEARABLE_DISABLE_FORCE_WEAR_PATH` and resets the phone side collector.

## Design Decisions (ADRs)
- **ADR 001 - Watch Collector Fallback**: The Watch requests to disable Collection on missed readings; the Phone makes the final determination based on local `Pref` values to maintain single-source-of-truth.
- **ADR 002 - WearOS Jitter Mitigation**: Watches targeted API 30+ (WearOS 3+) are implicitly treated with `JoH.buggy_samsung=true` parameters to fast-fail connections when Doze delays wakeups.
