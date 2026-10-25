# MeshCore CLI Commands Reference

A complete reference guide for all Command-Line Interface (CLI) commands supported by MeshCore nodes (Repeaters, Room Servers, Sensors, and Companion radios) over Serial, BLE, LoRa, or Ethernet TCP.

---

## Table of Contents

1. [Operational & System Commands](#1-operational--system-commands)
2. [Radio & RF Configuration](#2-radio--rf-configuration)
3. [Routing & Network Timing](#3-routing--network-timing)
4. [Advertisements & Discovery](#4-advertisements--discovery)
5. [Access Control & Security (ACL)](#5-access-control--security-acl)
6. [Region Management (Hierarchical Routing)](#6-region-management-hierarchical-routing)
7. [Power Management & Hardware Diagnostics](#7-power-management--hardware-diagnostics)
8. [Thermal & Fan Management](#8-thermal--fan-management)
9. [Sensors & GPIO](#9-sensors--gpio)
10. [GPS Location Services](#10-gps-location-services)
11. [Hardware Bridging (RS-232 & ESP-NOW)](#11-hardware-bridging-rs-232--esp-now)
12. [Ethernet Networking](#12-ethernet-networking)
13. [Statistics & Logging](#13-statistics--logging)
14. [Room Server Messaging](#14-room-server-messaging)
15. [Companion Radio, WiFi & CLI Rescue Mode](#15-companion-radio-wifi--cli-rescue-mode)

---

## 1. Operational & System Commands

| Command | Arguments | Access | Role / Scope | Description |
| :--- | :--- | :--- | :--- | :--- |
| `reboot` | None | All | All | Restarts the node hardware. *(Note: No reply is sent.)* |
| `poweroff` / `shutdown` | None | All | All | Powers off the node hardware. *(Note: No reply is sent.)* |
| `clkreboot` | None | All | All | Resets the real-time clock (RTC) back to the base default time (15 May 2024 20:50 UTC) and reboots. *(Note: No reply is sent.)* |
| `erase` | None | Serial Only | All | **Factory Reset:** Formats the node flash filesystem and wipes all configuration, contacts, and identities. |
| `ver` | None | All | All | Displays current firmware version and build date/timestamp (e.g. `v1.18.0 (Build: Jan 1 2026)`). |
| `board` | None | All | All | Displays the hardware manufacturer and target board name (e.g. `Heltec T096`, `Station G3`). |
| `get role` | None | All | All | Returns the node's running firmware role (e.g. `Repeater`, `Room Server`, `Sensor`). |
| `get name` | None | All | All | Displays the node's broadcast/advertised display name. |
| `set name <name>` | `<name>` (string) | All | All | Sets the node's display name. Names that exceed available advert space (23 bytes with location, 31 bytes without) are truncated at a valid UTF-8 boundary. Forbidden characters: `[`, `]`, `\`, `:`, `,`, `?`, `*`. |
| `clock` | None | All | All | Displays current node RTC date and time in UTC (`HH:MM - DD/MM/YYYY UTC`). |
| `clock sync` | None | All | All | Synchronizes node RTC clock with incoming message sender timestamp (forward-only; will not move clock backwards). |
| `time <epoch>` | `<epoch>` (seconds) | All | All | Manually sets RTC to Unix epoch timestamp in seconds (forward-only; cannot go backwards). |
| `password <pwd>` | `<pwd>` (string) | All | All | Changes the primary node admin password. The reply echoes the new password for confirmation. |
| `get owner.info` | None | All | All | Displays owner contact and description information. |
| `set owner.info <text>` | `<text>` (string) | All | All | Sets owner contact/info text. Use the `\|` character to insert line breaks. |
| `start ota` | None | All | Supported Boards | Puts supported boards into Over-The-Air (OTA) firmware update mode. |

---

## 2. Radio & RF Configuration

| Command | Arguments | Access | Description |
| :--- | :--- | :--- | :--- |
| `get radio` | None | All | Returns active LoRa parameters formatted as `> freq,bw,sf,cr`. Default preset: `869.618,62.5,8,5` (Narrow EU/UK). |
| `set radio <freq>,<bw>,<sf>,<cr>` | Frequency (MHz), Bandwidth (kHz), Spreading Factor (5-12), Coding Rate (5-8) | All | Sets LoRa radio parameters in flash. Requires a `reboot` to apply. Valid ranges: `freq` 150.0-2500.0 MHz, `bw` 7.0-500.0 kHz, `sf` 5-12, `cr` 5-8. |
| `tempradio <freq>,<bw>,<sf>,<cr>,<mins>` | Radio params + duration in minutes | All | Temporarily applies LoRa parameters in RAM for `<mins>` without saving to flash. Reverts automatically after timer expiration or on reboot. |
| `get freq` | None | All | Displays configured radio frequency in MHz (default `869.618`). |
| `set freq <mhz>` | Frequency in MHz | Serial Only | Changes radio frequency directly in flash. Requires a `reboot` to apply. |
| `get tx` | None | All | Displays configured LoRa RF transmit power level in dBm. |
| `set tx <dbm>` | Power in dBm (-9 to 30) | All | Sets LoRa RF transmit power in dBm. Applied immediately to the radio driver. |
| `get radio.rxgain` | None | All | Shows whether internal SX126x/LR1110 boosted RX gain is active (`on` or `off`). |
| `set radio.rxgain <on\|off>` | `on` or `off` | All | Enables or disables internal SX126x/LR1110 chip boosted RX gain mode. |
| `get radio.fem.rxgain` | None | All | Shows external LoRa Front-End Module (FEM) LNA RX gain (`on`, `off`, or `Error: unsupported`). Supported on Heltec T096, Heltec V4, Heltec V4 R8, Heltec Tracker V2, Station G3. |
| `set radio.fem.rxgain <on\|off>` | `on` or `off` | All | Enables or disables external LoRa FEM receive-path LNA gain on supported boards. |
| `get radio.fem.txgain` | None | All | Shows external LoRa Front-End Module (FEM) Power Amplifier (PA) transmit gain (`on`, `off`, or `Error: unsupported`). Supported on Station G3 ESP32, Wio Tracker L1 1W. |
| `set radio.fem.txgain <on\|off>` | `on` or `off` | All | Enables or disables external LoRa FEM transmit-path PA gain on supported boards (e.g. Station G3, Wio Tracker L1 1W). Applied at the start of the next transmission. |
| `get extra.sf` | None | All | Displays configured extra side-detector Spreading Factors on transceivers with multi-SF detection support (e.g. LR2021). |
| `set extra.sf <sf1,sf2,...>` | Up to 3 comma-separated SFs | All | Configures up to 3 extra side-detector Spreading Factors for concurrent packet detection (e.g. LR2021). |
| `get cad` | None | All | Returns whether hardware Channel Activity Detection (CAD) is enabled (`on` or `off`). |
| `set cad <on\|off>` | `on` or `off` | All | Enables or disables Channel Activity Detection before packet transmissions. Defers transmission when channel is busy. |
| `get agc.reset.interval` | None | All | Displays the receiver AGC reset interval in seconds. |
| `set agc.reset.interval <S>` | Interval in seconds (`0` to `1020`) | All | Sets AGC reset interval (automatically rounded down to a multiple of 4 seconds; `0` disables). Clamped to 0-1020 seconds. |

---

## 3. Routing & Network Timing

| Command | Arguments | Access | Description |
| :--- | :--- | :--- | :--- |
| `get repeat` | None | All | Shows whether packet forwarding/repeating is active (`on` or `off`). |
| `set repeat <on\|off>` | `on` or `off` | All | Enables or disables repeating and mesh packet retransmission. *(Note: On Room Servers, `set repeat on` is deprecated and returns `ERR - repeat now deprecated`.)* |
| `get dutycycle` | None | All | Displays maximum allowed airtime duty cycle as a percentage (e.g. `> 50.0%`). Added in v1.15.0. |
| `set dutycycle <1-100>` | Percentage (1 to 100) | All | Restricts airtime transmission duty cycle to comply with regional limits (e.g. `10` for 10% EU 868 MHz, `1` for 1% EU limit, `50` for default 50%, `100` for unlimited). Refuses non-numeric/`nan` values. |
| `get af` | None | All | Displays internal airtime factor value (`100/dc - 1`). **Deprecated** as of v1.15.0; use `dutycycle`. |
| `set af <factor>` | Float factor (0.0 to 9.0) | All | Configures airtime factor directly. Refuses non-numeric/`nan` values. **Deprecated** as of v1.15.0; use `set dutycycle`. |
| `get rxdelay` | None | All | Displays the weak-signal receive delay base factor. |
| `set rxdelay <0-20>` | Float (0.0 to 20.0) | All | Sets base delay factor for incoming packets. Packets received with strong signals process immediately, while weak-signal packets are held in a delay queue so duplicates can be suppressed. `0` disables. |
| `get txdelay` | None | All | Displays the flood transmit contention delay multiplier factor (default `0.5`). |
| `set txdelay <0-2>` | Float (0.0 to 2.0) | All | Sets packet transmit contention delay multiplier to mitigate collisions when multiple nearby repeaters retransmit. `0` disables random window. |
| `get direct.txdelay` | None | All | Displays the direct message transmit delay factor (default `0.3` on Repeater, `0.2` on Room Server & Sensor). |
| `set direct.txdelay <0-2>` | Float (0.0 to 2.0) | All | Sets transmit delay factor specifically for direct (non-flood, routed) unicast packets. |
| `get flood.max` | None | All | Displays max flood hop count for standard traffic (default `64`). |
| `set flood.max <0-64>` | Max hops (0 to 64) | All | Configures maximum hop count allowed for standard flood packets. |
| `get flood.max.advert` | None | All | Displays max flood hop count allowed for node advertisements (default `8`). |
| `set flood.max.advert <0-64>` | Max hops (0 to 64) | All | Sets maximum hop count for self-advertisements across the network. |
| `get flood.max.unscoped` | None | All | Displays max flood hop count for unscoped mesh packets (default `64`). |
| `set flood.max.unscoped <0-64>` | Max hops (0 to 64) | All | Restricts flood propagation reach for packets lacking an explicit region scope. |
| `get loop.detect` | None | All | Displays loop detection mode (`off`, `minimal`, `moderate`, `strict`). Default is `minimal` on repeaters (`off` on other roles). |
| `set loop.detect <mode>` | `off`, `minimal`, `moderate`, `strict` | All | Configures loop prevention filter sensitivity to discard duplicate re-broadcast loops. |
| `get path.hash.mode` | None | All | Shows path hashing mode: `0` = 1-Byte/8-bit, `1` = 2-Byte/16-bit, `2` = 3-Byte/24-bit. |
| `set path.hash.mode <0\|1\|2>` | `0`, `1`, or `2` | All | Sets hash size used for path tracking in self-advertisement packet headers: `0` (1-byte hash, 256 IDs, max 64 hops), `1` (2-byte hash, 65,536 IDs, max 32 hops), `2` (3-byte hash, 16.7M IDs, max 21 hops). |
| `get int.thresh` | None | All | Displays interference threshold RSSI/SNR setting. |
| `set int.thresh <val>` | Integer value | All | Configures RF interference detection threshold. Transmissions defer if background noise exceeds this threshold. |
| `get multi.acks` | None | All | Displays whether Multi-ACKs support is enabled (`> 0` or `> 1`). |
| `set multi.acks <0\|1>` | `0` (disable) or `1` (enable) | All | Enables or disables Multi-ACK support for reliable redundant message delivery confirmation. |

---

## 4. Advertisements & Discovery

| Command | Arguments | Access | Role / Scope | Description |
| :--- | :--- | :--- | :--- | :--- |
| `advert` | None | All | All | Immediately broadcasts a flooded node self-advertisement across the mesh network. |
| `advert.zerohop` | None | All | All | Broadcasts an immediate zero-hop (local only, non-forwarded) node advertisement. |
| `get advert.interval` | None | All | All | Displays interval (minutes) between automatic local zero-hop advertisements (reports `2` on factory-fresh node, then `0` once configured). |
| `set advert.interval <mins>` | Minutes (60-240 or `0`) | All | All | Sets how often the node transmits automatic local zero-hop adverts (range 60-240 minutes, rounded down to even multiple; `0` disables). Saving any setting resets an interval below 60 to `0`. |
| `get flood.advert.interval` | None | All | All | Displays interval (hours) between automatic flood advertisements (default `47` on Repeater & Room Server, `0` disabled on Sensor). |
| `set flood.advert.interval <hrs>`| Hours (3 to 168 or `0`) | All | All | Sets how often the node floods an advertisement across the entire network (range 3-168 hours; `0` disables). |
| `neighbors` | None | All | Repeater Only | Displays up to 8 of the most recently heard direct neighbor nodes formatted as `{pubkey_prefix}:{timestamp}:{snr*4}`. |
| `neighbor.remove <prefix>` | `<pubkey_prefix>` or space | All | Repeater Only | Removes matching neighbor(s) from table. Passing a single space character clears all neighbors. |
| `discover.neighbors` | None | All | Repeater Only | Transmits a zero-hop neighbor discovery request packet to prompt nearby repeaters/nodes to reply. *(Note: Hidden repeaters with adverts turned off do not reply).* |

---

## 5. Access Control & Security (ACL)

| Command | Arguments | Access | Role / Scope | Description |
| :--- | :--- | :--- | :--- | :--- |
| `get public.key` | None | All | All | Displays the node's 32-byte public identity key in hexadecimal format. |
| `get prv.key` | None | Serial Only | All | Displays the node's 32-byte private identity key in hexadecimal format. |
| `set prv.key <hex>` | 32-byte hex key (64 hex chars) | All (Admin) | All | Changes node private identity key (rekeys identity). Requires a `reboot` to apply. |
| `get acl` | None | Serial Only | Repeater, Server, Sensor | Dumps the full Access Control List containing permissions byte and companion public keys. |
| `setperm <pubkey> <perm>` | Hex Public Key + Perm (`0`-`3`) | All (Admin) | Repeater, Server, Sensor | Sets ACL permission level for a companion public key: `0` = Guest (or removes custom perm), `1` = Read-only, `2` = Read-write, `3` = Admin. |
| `get guest.password` | None | All | Room Server | Displays the current guest access password. |
| `set guest.password <pwd>` | `<pwd>` (string) | All (Admin) | Room Server | Configures password required for guest-tier room access. |
| `get allow.read.only` | None | All | Room Server | Shows whether unauthenticated read-only access is permitted (`on` or `off`). |
| `set allow.read.only <on\|off>` | `on` or `off` | All (Admin) | Room Server | Enables or disables read-only guest queries without credentials. |

---

## 6. Region Management (Hierarchical Routing)

*(Supported on Repeaters, Room Servers, and Sensors)*

| Command | Arguments | Access | Description |
| :--- | :--- | :--- | :--- |
| `region` | None | All | Exports the current hierarchical region map definition. |
| `region def <definition>` | Space-separated tokens (`name` or `name\|jump`) | All | Defines and builds a complete region hierarchy tree in a single command. Supports cursor jumping (`\|jump` or `,jump`). Review with `region` and commit with `region save`. |
| `region put <name> [parent]` | Region name + optional parent prefix | All | Inserts a new region into the tree under the given parent (defaults to root wildcard `*`). Flooding is allowed by default. |
| `region remove <name>` | Region name | All | Removes a region from the tree (region must be a leaf with no child regions). |
| `region allowf <name>` | Region name prefix (or `*`) | All | Permits flood packet propagation within the specified region (`*` allows unscoped packets). |
| `region denyf <name>` | Region name prefix (or `*`) | All | Blocks/denies flood packet propagation within the specified region (`*` drops unscoped packets). |
| `region get <name>` | Region name prefix (or `*`) | All | Displays region details: name, parent region name, and flood permission flag (`F` if allowed, blank if denied). |
| `region home` | None | All | Displays the current home region (`*` if unset). |
| `region home <name>` | Region name prefix | All | Sets the node's designated home region. |
| `region default` | None | All | Displays current default scope region (`<null>` if unset). |
| `region default <name\|<null>>`| Region name prefix or `<null>` | All | Sets default scope applied to outbound unscoped packets (auto-creates region if missing and enables flood), or `<null>` to clear. Persists atomically. |
| `region list allowed` | None | All | Lists all regions where mesh flooding is currently permitted. |
| `region list denied` | None | All | Lists all regions where mesh flooding is currently denied. |
| `region load` | Multiline region tree input | All | Begins interactive batch loading of region definitions (indentation denotes parent-child relationships; terminate with a blank line). |
| `region save` | None | All | Commits and persists the region hierarchy into non-volatile storage. |

---

## 7. Power Management & Hardware Diagnostics

| Command | Arguments | Access | Role / Scope | Description |
| :--- | :--- | :--- | :--- | :--- |
| `power` | None | All | All | Displays comprehensive power subsystem diagnostics: `batt:<mV> mV usb:<yes\|no\|n/a> solar_chg:<yes\|no\|n/a> ext:<yes\|no> charger:<yes\|no>`. |
| `powersaving` | None | All | Repeater Only | Displays whether low-power sleep mode is active (`on` or `off`). |
| `powersaving on` | None | All | Repeater Only | Activates low-power sleep mode (immediate effect on nRF52; after 2-minute delay on ESP32; unsupported on bridge builds). |
| `powersaving off` | None | All | Repeater Only | Deactivates low-power sleep mode (keeps node radio and CPU awake). |
| `get pwrmgt.support` | None | All | Supported Boards | Displays whether advanced power management is supported (`> supported` or `> unsupported`). |
| `get pwrmgt.source` | None | All | Supported Boards | Returns current active power source (`> external` or `> battery`). |
| `get pwrmgt.bootreason` | None | All | Supported Boards | Returns hardware reset reason and last shutdown reason strings. |
| `get pwrmgt.bootmv` | None | All | Supported Boards | Returns battery / input voltage measured during boot in millivolts (e.g. `> 4150 mV`). |
| `get adc.multiplier` | None | All | Supported Boards | Displays the ADC voltage divider calibration multiplier for battery sensing. |
| `set adc.multiplier <mult>` | Float value (`0` for default) | All | Supported Boards | Calibrates battery ADC multiplier (`0` reverts to compiled board default). |
| `get bootloader.ver` | None | All | nRF52 Boards | Displays the nRF52 UF2/DFU bootloader version string. |

---

## 8. Thermal & Fan Management

*(Available on boards with active cooling / thermal sensors, e.g. LilyGo T-Beam 1W)*

| Command | Arguments | Access | Role / Scope | Description |
| :--- | :--- | :--- | :--- | :--- |
| `get fan` | None | All | T-Beam 1W | Displays current fan mode (`auto`, `on`, `off`), measured PA temperature in °C, actual fan state (`on` or `off`), and TX cooldown timer (e.g. `> auto 41.2C fan=off cd=0s`). |
| `set fan <on\|off\|auto>` | `on`, `off`, or `auto` | All | T-Beam 1W | Sets PA cooling fan operating mode. |
| `set fan.lo <temp_c>` | Integer (0 to 100, must be < `fan.hi`) | All | T-Beam 1W | Sets the lower temperature threshold in °C. In `auto` mode, the fan turns off when temperature drops below this threshold. |
| `set fan.hi <temp_c>` | Integer (> `fan.lo` and <= 120) | All | T-Beam 1W | Sets the upper temperature threshold in °C. In `auto` mode, the fan turns on when temperature exceeds this threshold. |

---

## 9. Sensors & GPIO

| Command | Arguments | Access | Role / Scope | Description |
| :--- | :--- | :--- | :--- | :--- |
| `sensor list [start]` | Optional start index | All | Sensor & Telemetry | Lists all configured sensor telemetry variables (`<name>=<value>`). |
| `sensor get <key>` | Setting key name | All | Sensor & Telemetry | Retrieves the current value of a custom sensor setting. |
| `sensor set <key> <value>` | Key name + value | All | Sensor & Telemetry | Updates the value of a custom sensor configuration variable. |
| `io` | None | All | Sensor Role | Reads and returns current GPIO state in hexadecimal. |
| `io <hex_val>` | Hex value | All | Sensor Role | Directly writes the full GPIO register. |
| `io s <hex_mask>` | Hex bitmask | All | Sensor Role | Sets specific GPIO pin bits HIGH without altering others. |
| `io r <hex_mask>` | Hex bitmask | All | Sensor Role | Resets specific GPIO pin bits LOW without altering others. |
| `io t <hex_mask>` | Hex bitmask | All | Sensor Role | Toggles the state of specific GPIO pin bits. |

---

## 10. GPS Location Services

*(Available when GPS support is compiled into firmware)*

| Command | Arguments | Access | Description |
| :--- | :--- | :--- | :--- |
| `gps` | None | All | Displays GPS operational status, fix status, and visible satellite count (e.g. `on, active, fix, 12 sats` or `off`). |
| `gps diag` | None | All | Dumps low-level GPS diagnostics: requested state, sentence counts, antenna status, and fix quality from the active location provider. |
| `gps on` | None | All | Powers up the GPS module and enables NMEA parsing. |
| `gps off` | None | All | Powers down GPS hardware to conserve battery power. |
| `gps sync` | None | All | Synchronizes node RTC time with GPS satellite clock. |
| `gps setloc` | None | All | Copies current live GPS fix coordinates into static `lat` and `lon` preferences. |
| `get lat` / `set lat <lat>` | Latitude in degrees (float) | All | Views or manually sets node static latitude. |
| `get lon` / `set lon <lon>` | Longitude in degrees (float) | All | Views or manually sets node static longitude. |
| `gps advert` | None | All | Displays current location sharing policy for node advertisements (`> none`, `> share`, or `> prefs`). |
| `gps advert <policy>` | `none`, `share`, `prefs` | All | Configures GPS location sharing in adverts (`none` omits location, `share` uses live GPS fix, or `prefs` uses static `lat`/`lon`). |

---

## 11. Hardware Bridging (RS-232 & ESP-NOW)

*(Available on firmware compiled with bridge support)*

| Command | Arguments | Access | Description |
| :--- | :--- | :--- | :--- |
| `get bridge.type` | None | All | Displays compiled bridge hardware interface (`rs232`, `espnow`, or `none`). |
| `get bridge.enabled` | None | All | Shows whether packet bridging is active (`on` or `off`). |
| `set bridge.enabled <on\|off>`| `on` or `off` | All | Enables or disables hardware packet bridge forwarding. |
| `get bridge.delay` | None | All | Displays bridge packet forwarding delay in milliseconds (default `500`). |
| `set bridge.delay <ms>` | Delay (0-10000 ms) | All | Configures delay injected before transmitting bridged packets. |
| `get bridge.source` | None | All | Shows packet bridge source stream (`logRx` or `logTx`). |
| `set bridge.source <src>` | `logRx` or `logTx` (or `rx`/`tx`) | All | Sets whether the bridge forwards received (`logRx`) or transmitted (`logTx`) packets. |
| `get bridge.baud` | None | All | (RS-232 only) Displays serial bridge baud rate. |
| `set bridge.baud <rate>` | 9600 to 115200 | All | (RS-232 only) Configures serial bridge baud rate. |
| `get bridge.channel` | None | All | (ESP-NOW only) Displays WiFi channel used for bridge. |
| `set bridge.channel <1-14>` | Channel (1-14) | All | (ESP-NOW only) Sets WiFi channel for ESP-NOW bridging. |
| `get bridge.secret` | None | All | (ESP-NOW only) Displays ESP-NOW authentication secret key. |
| `set bridge.secret <secret>` | String up to 15 chars | All | (ESP-NOW only) Sets shared secret for ESP-NOW bridge pairing. |

---

## 12. Ethernet Networking

*(Available on boards with Ethernet controllers, e.g. RAK4631 + RAK13800)*

| Command | Arguments | Access | Role / Scope | Description |
| :--- | :--- | :--- | :--- | :--- |
| `eth.status` | None | All | Repeater, Room Server | Returns Ethernet connection status and Telnet CLI endpoint (e.g. `ETH: 192.168.1.50:23` or `ETH: not connected`). |

---

## 13. Statistics & Logging

| Command | Arguments | Access | Description |
| :--- | :--- | :--- | :--- |
| `stats-core` | None | Serial Only | Dumps system metrics: battery voltage, system uptime, task queue length, and debug flags. |
| `stats-radio` | None | Serial Only | Dumps RF metrics: noise floor, last packet RSSI/SNR, transmission airtime, and receive CRC errors. |
| `stats-packets` | None | Serial Only | Dumps packet traffic diagnostic counters: total received, transmitted, repeated/forwarded, and dropped. |
| `clear stats` | None | All | Resets core, radio, and packet diagnostic counters back to zero. |
| `log start` | None | All | Starts capturing incoming packet traffic logs to onboard flash storage. |
| `log stop` | None | All | Stops active flash logging. |
| `log erase` | None | All | Erases captured packet log file from flash storage. |
| `log` | None | Serial Only | Dumps full content of the captured flash log file over serial connection. |

---

## 14. Room Server Messaging

*(Available on Room Server firmware)*

| Command | Arguments | Access | Role / Scope | Description |
| :--- | :--- | :--- | :--- | :--- |
| `room.post <message>` | `<message>` (text) | All (Admin) | Room Server Only | Injects an administrative/system post directly into the room server chat history. |

---

## 15. Companion Radio, WiFi & CLI Rescue Mode

*(Available on Companion Radio firmware over Serial/BLE/TCP and emergency CLI Rescue)*

### Companion Radio Commands

| Command | Arguments | Access | Description |
| :--- | :--- | :--- | :--- |
| `get tz.offset` | None | All | Displays configured timezone offset in hours relative to UTC (e.g. `> 5.5`, `> -5.0`). |
| `set tz.offset <offset>` | Float (`-12.0` to `14.0`) | All | Configures local timezone offset for companion radio clock and UI. Accepts fractional hours (e.g. `5.5`, `9.5`, `12.75`, `-3.5`). |
| `set pin <pin>` | 6-digit integer | All | Configures Bluetooth LE (BLE) PIN required for pairing companion devices (e.g. `set pin 123456`). |
| `reboot` | None | All | Restarts the companion radio hardware. |
| `poweroff` / `shutdown` | None | All | Powers down the companion radio hardware. |

### Companion WiFi Configuration

*(Available on companion radio builds with WiFi support, e.g. ESP32 variants, Raspberry Pi Pico W)*

| Command | Arguments | Access | Description |
| :--- | :--- | :--- | :--- |
| `get wifi.ssid` | None | All | Displays configured WiFi network SSID (returns `> (not set)` if blank). |
| `set wifi.ssid <ssid>` | SSID string | All | Sets WiFi network SSID. Set `wifi.pwd` as well, then reboot to apply. |
| `get wifi.pwd` | None | All | Displays configured WiFi network password. |
| `set wifi.pwd <pwd>` | Password string | All | Sets WiFi network password (reboot to apply). |
| `set wifi.clear` | None | All | Clears stored WiFi SSID and password (reboot to apply). |
| `get wifi.enabled` | None | All | Displays whether WiFi interface is enabled (`1`) or disabled (`0`). |
| `set wifi.enabled <0\|1>` | `0` or `1` | All | Enables (`1`) or disables (`0`) the WiFi network interface (reboot to apply). |
| `get wifi.status` | None | All | Returns active WiFi connection status (`> connected` or `> disconnected`). |
| `get wifi.ip` | None | All | Returns local IP address when connected (e.g. `> 192.168.1.105`) or `> (not connected)`. |

### CLI Rescue Mode

*(Available via Serial Console during emergency recovery mode on Companion Radios)*

| Command | Arguments | Access | Description |
| :--- | :--- | :--- | :--- |
| `rebuild` | None | Serial (CLI Rescue) | Formats filesystem and restores core identity, preferences, contacts, and channels. |
| `erase` | None | Serial (CLI Rescue) | Formats and wipes the companion radio filesystem. |
| `ls [path]` | Optional directory path | Serial (CLI Rescue) | Lists files and directories stored on primary (`UserData/`) or secondary (`ExtraFS/`) filesystems. |
| `cat <path>` | Path in `UserData/` or `ExtraFS/` | Serial (CLI Rescue) | Dumps raw contents of specified file as hex bytes to the serial console. |
| `rm <path>` | Path in `UserData/` or `ExtraFS/` | Serial (CLI Rescue) | Permanently deletes a specified file from storage. |
| `reboot` | None | Serial (CLI Rescue) | Restarts the companion radio hardware. |
