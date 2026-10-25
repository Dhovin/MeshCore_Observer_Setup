# MeshCore Supported Hardware Reference

This document provides a comprehensive inventory of all **92 hardware platforms and board variants** supported by the MeshCore firmware, complete with microcontroller architecture, LoRa transceivers, connectivity interfaces, onboard peripherals, and target build environments.

---

## Table of Contents

- [1. Hardware Overview & Architectures](#1-hardware-overview--architectures)
- [2. Supported Roles & Firmware Types](#2-supported-roles--firmware-types)
- [3. Hardware Catalog by Ecosystem](#3-hardware-catalog-by-ecosystem)
  - [B-Calm (3 boards)](#b-calm)
  - [Ebyte (1 boards)](#ebyte)
  - [Elecrow (9 boards)](#elecrow)
  - [GAT562 (4 boards)](#gat562)
  - [Generic / DIY (2 boards)](#generic--diy)
  - [Heltec Automation (18 boards)](#heltec-automation)
  - [Ikoka (3 boards)](#ikoka)
  - [KeepTeen (1 boards)](#keepteen)
  - [LilyGO (14 boards)](#lilygo)
  - [M5Stack (1 boards)](#m5stack)
  - [Maker DIY (4 boards)](#maker-diy)
  - [Meshnology (1 boards)](#meshnology)
  - [MinewSemi (1 boards)](#minewsemi)
  - [Muzi Works (2 boards)](#muzi-works)
  - [Nibble (2 boards)](#nibble)
  - [RAKwireless (6 boards)](#rakwireless)
  - [Raspberry Pi (1 boards)](#raspberry-pi)
  - [Seeed Studio (16 boards)](#seeed-studio)
  - [Tenstar (1 boards)](#tenstar)
  - [Waveshare (2 boards)](#waveshare)
- [4. Architecture Quick Reference Matrix](#4-architecture-quick-reference-matrix)
- [5. Flashing and Building](#5-flashing-and-building)

---

## 1. Hardware Overview & Architectures

MeshCore is designed to run across multiple silicon architectures to balance compute power, battery efficiency, and cost:

| Architecture / SoC | Core | Connectivity | Best For |
| :--- | :--- | :--- | :--- |
| **Nordic nRF52840** | ARM Cortex-M4F @ 64MHz | BLE 5.0, USB | Ultra-low power solar repeaters, battery companions, wearables |
| **Espressif ESP32 / S3** | Xtensa Dual-Core up to 240MHz | Wi-Fi, BLE, USB | High-throughput nodes, Room Servers, ESP-NOW bridges, touch displays |
| **Espressif ESP32-C3 / C6** | RISC-V Single-Core up to 160MHz | Wi-Fi 6, BLE 5, USB | Modern compact nodes, low power Wi-Fi repeating |
| **Raspberry Pi RP2040** | ARM Cortex-M0+ Dual-Core @ 133MHz | USB (Opt. Wi-Fi on Pico W) | Modular systems (RAK WisBlock, Waveshare, Pico W) |
| **STMicroelectronics STM32WLE5** | ARM Cortex-M4 + Integrated LoRa | USB / UART | Ultra-compact coin-cell relays and sensor nodes |

---

## 2. Supported Roles & Firmware Types

Depending on the board features, each hardware target supports one or more of the following firmware roles:

- **Repeater (`repeater`)**: Dedicated routing node that relays packets across the mesh, maintains neighbor tables, and supports remote CLI management.
- **Companion Radio (`companion_radio_ble` / `usb` / `wifi`)**: Wireless or wired modem for smartphone apps (Android / iOS) and web clients (e.g. `app.meshcore.nz`).
- **Room Server (`room_server`)**: Decentralized BBS server storing public message boards, posts, and announcements.
- **Sensor (`sensor`)**: Low-power telemetry node broadcasting periodic sensor readings and accepting remote GPIO control.
- **KISS Modem (`kiss_modem`)**: Standard serial KISS modem for amateur radio software, APRS, and packet networks.
- **Hardware Bridge (`bridge_rs232` / `bridge_espnow`)**: Dual-radio or serial bridge interconnecting independent mesh segments.

---

## 3. Hardware Catalog by Ecosystem

### B-Calm

*3 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Nano G2 Ultra**<br>`variants/nano_g2_ultra` | Nordic nRF52840 | SX1262 | BLE, USB | Rugged aluminum case, Internal antenna, Low power | 5 envs (e.g. `Nano_G2_Ultra_repeater`, `Nano_G2_Ultra_room_server`) |
| **Station G2 Repeater**<br>`variants/station_g2` | ESP32 | SX1262 | Wi-Fi, BLE, USB | Rugged outdoor repeater, High-capacity battery, Solar | 9 envs (e.g. `Station_G2_repeater`, `Station_G2_repeater_bridge_espnow`) |
| **Station G3 ESP32**<br>`variants/station_g3_esp32` | ESP32 | SX1262 | Wi-Fi, BLE, USB | Next-gen outdoor base station, High RF sensitivity | 7 envs (e.g. `Station_G3_ESP32_repeater`, `Station_G3_ESP32_logging_repeater`) |

---

### Ebyte

*1 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Ebyte EORA-S3**<br>`variants/ebyte_eora_s3` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Compact module with integrated antenna | 6 envs (e.g. `Ebyte_EoRa-S3_Repeater`, `Ebyte_EoRa-S3_terminal_chat`) |

---

### Elecrow

*9 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **ThinkNode M1**<br>`variants/thinknode_m1` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Portable handheld communicator with display | 5 envs (e.g. `ThinkNode_M1_repeater`, `ThinkNode_M1_room_server`) |
| **ThinkNode M2**<br>`variants/thinknode_m2` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Outdoor repeater station with solar charging | 9 envs (e.g. `ThinkNode_M2_Repeater`, `ThinkNode_M2_Repeater_bridge_espnow`) |
| **ThinkNode M3**<br>`variants/thinknode_m3` | Nordic nRF52840 | SX1262 | BLE, USB | Ultra-low power pocket node | 5 envs (e.g. `ThinkNode_M3_repeater`, `ThinkNode_M3_room_server`) |
| **ThinkNode M4**<br>`variants/thinknode_m4` | Nordic nRF52840 | Semtech LR1110 | BLE, USB | Solar charging, GNSS positioning, multi-band transceiver, low power | 5 envs (e.g. `ThinkNode_M4_repeater`, `ThinkNode_M4_companion_radio_ble`) |
| **ThinkNode M5**<br>`variants/thinknode_m5` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Desktop terminal node with screen | 9 envs (e.g. `ThinkNode_M5_Repeater`, `ThinkNode_M5_Repeater_bridge_espnow`) |
| **ThinkNode M6**<br>`variants/thinknode_m6` | Nordic nRF52840 | SX1262 | BLE, USB | Wearable companion node | 5 envs (e.g. `ThinkNode_M6_repeater`, `ThinkNode_M6_room_server`) |
| **ThinkNode M7**<br>`variants/thinknode_m7` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Industrial sensor and telemetry hub | 7 envs (e.g. `ThinkNode_M7_repeater`, `ThinkNode_M7_room_server`) |
| **ThinkNode M8**<br>`variants/thinknode_m8` | Nordic nRF52840 | SX1262 | BLE, USB | Miniature tracker with GNSS | 5 envs (e.g. `ThinkNode_M8_repeater`, `ThinkNode_M8_room_server`) |
| **ThinkNode M9**<br>`variants/thinknode_m9` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Long-range base station | 6 envs (e.g. `ThinkNode_M9_repeater_`, `ThinkNode_M9_room_server_`) |

---

### GAT562

*4 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **GAT562 30S Mesh Kit**<br>`variants/gat562_30s_mesh_kit` | Nordic nRF52840 | SX1262 (30dBm PA) | BLE, USB | GPS, 0.96" OLED, Battery | 5 envs (e.g. `GAT562_30S_Mesh_Kit_repeater`, `GAT562_30S_Mesh_Kit_room_server`) |
| **GAT562 EVB Pro**<br>`variants/gat562_mesh_evb_pro` | Nordic nRF52840 | SX1268 | BLE, USB | 0.96" OLED, Rotary Encoder, Battery | `GAT562_Mesh_EVB_Pro_repeater`, `GAT562_Mesh_EVB_Pro_room_server`, `GAT562_Mesh_EVB_Pro_kiss_modem` |
| **GAT562 Mesh Tracker Pro**<br>`variants/gat562_mesh_tracker_pro` | Nordic nRF52840 | SX1262 | BLE, USB | GPS, 0.96" OLED, Battery | 5 envs (e.g. `GAT562_Mesh_Tracker_Pro_repeater`, `GAT562_Mesh_Tracker_Pro_room_server`) |
| **GAT562 Mesh Watch 13**<br>`variants/gat562_mesh_watch13` | Nordic nRF52840 | SX1262 | BLE, USB | Smartwatch 1.3" TFT, Battery, Accelerometer | `GAT562_Mesh_Watch13_companion_radio_ble`, `GAT562_Mesh_Watch13_kiss_modem` |

---

### Generic / DIY

*2 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Generic ESP32 + Ebyte E22**<br>`variants/generic-e22` | ESP32 | E22 (SX1262/SX1268) | Wi-Fi, BLE, USB | Custom Wiring, High-Power PA | 5 envs (e.g. `Generic_E22_sx1262_repeater`, `Generic_E22_sx1262_repeater_bridge_espnow`) |
| **Generic ESP32-S3 (ESP-NOW Bridge)**<br>`variants/generic_espnow` | ESP32-S3 | SX1262 | Wi-Fi (ESP-NOW), BLE, USB | Wireless bridge between subnets | `Generic_ESPNOW_terminal_chat`, `Generic_ESPNOW_repeatr`, `Generic_ESPNOW_comp_radio_usb`, `Generic_ESPNOW_room_svr` |

---

### Heltec Automation

*18 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Heltec CT62**<br>`variants/heltec_ct62` | ESP32-C3 | SX1262 | Wi-Fi, BLE, USB | Ultra-compact stamp module | 6 envs (e.g. `Heltec_ct62_repeater`, `Heltec_ct62_repeater_bridge_espnow`) |
| **Heltec Mesh Node T114**<br>`variants/heltec_t114` | Nordic nRF52840 | SX1262 | BLE, USB | 1.14-inch TFT Color Display, Optional GPS, LiPo Charger | 11 envs (e.g. `Heltec_t114_without_display_repeater`, `Heltec_t114_without_display_repeater_bridge_rs232`) |
| **Heltec Mesh Pocket**<br>`variants/mesh_pocket` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Pocket-sized enclosed messenger with battery | 5 envs (e.g. `Mesh_pocket_repeater`, `Mesh_pocket_room_server`) |
| **Heltec Mesh Solar Node**<br>`variants/heltec_mesh_solar` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Solar charging controller, Weatherproof enclosure | 5 envs (e.g. `Heltec_mesh_solar_repeater`, `Heltec_mesh_solar_room_server`) |
| **Heltec RC32**<br>`variants/heltec_rc32` | ESP32-C3 | SX1262 | Wi-Fi, BLE, USB | Compact receiver node | 16 envs (e.g. `heltec_rc32_without_display_repeater`, `heltec_rc32_without_display_repeater_bridge_espnow`) |
| **Heltec T1**<br>`variants/heltec_t1` | Nordic nRF52840 | SX1262 | BLE, USB | Ultra-low power node | 5 envs (e.g. `Heltec_t1_repeater`, `Heltec_t1_room_server`) |
| **Heltec Tower V2**<br>`variants/heltec_tower_v2` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Outdoor repeater station, Solar/PoE options | 5 envs (e.g. `Heltec_tower_v2_repeater`, `Heltec_tower_v2_room_server`) |
| **Heltec Vision Master T096**<br>`variants/heltec_t096` | Nordic nRF52840 | SX1262 | BLE, USB | 0.96-inch OLED Display, External FEM/LNA | 7 envs (e.g. `Heltec_t096_repeater`, `Heltec_t096_repeater_bridge_rs232`) |
| **Heltec Vision Master T190**<br>`variants/heltec_t190` | Nordic nRF52840 | SX1262 | BLE, USB | 1.90-inch Display, Ultra-low power sleep | 6 envs (e.g. `Heltec_T190_companion_radio_ble_`, `Heltec_T190_companion_radio_usb_`) |
| **Heltec WiFi LoRa 32 V2**<br>`variants/heltec_v2` | ESP32 | SX1276 | Wi-Fi, BLE, USB | 0.96-inch OLED, LiPo battery charger | 8 envs (e.g. `Heltec_v2_repeater`, `Heltec_v2_repeater_bridge_espnow`) |
| **Heltec WiFi LoRa 32 V3**<br>`variants/heltec_v3` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | 0.96-inch OLED, LiPo battery charger, Type-C | 18 envs (e.g. `Heltec_v3_repeater`, `Heltec_v3_repeater_bridge_rs232`) |
| **Heltec WiFi LoRa 32 V4**<br>`variants/heltec_v4` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | OLED, Power optimization, Expansion kit support | 18 envs (e.g. `heltec_v4_repeater`, `heltec_v4_expansionkit_repeater`) |
| **Heltec WiFi LoRa 32 V4-R8**<br>`variants/heltec_v4_r8` | ESP32-S3 (8MB PSRAM) | SX1262 | Wi-Fi, BLE, USB | High-RAM variant for heavy room servers/routing | 16 envs (e.g. `heltec_v4_r8_repeater`, `heltec_v4_r8_room_server`) |
| **Heltec Wireless Paper**<br>`variants/heltec_wireless_paper` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | 2.13-inch Tri-color / Mono E-Paper, Battery | 6 envs (e.g. `Heltec_Wireless_Paper_companion_radio_ble`, `Heltec_Wireless_Paper_companion_radio_usb`) |
| **Heltec Wireless Paper E213**<br>`variants/heltec_e213` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | 2.13-inch E-Ink Display, Battery | 6 envs (e.g. `Heltec_E213_companion_radio_ble`, `Heltec_E213_companion_radio_usb`) |
| **Heltec Wireless Paper E290**<br>`variants/heltec_e290` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | 2.9-inch E-Ink Display, Battery | 6 envs (e.g. `Heltec_E290_companion_ble`, `Heltec_E290_companion_usb`) |
| **Heltec Wireless Tracker V1.1**<br>`variants/heltec_tracker` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | 0.96-inch TFT, GNSS/GPS (UC6580), LiPo charger | 6 envs (e.g. `Heltec_Wireless_Tracker_companion_radio_usb`, `Heltec_Wireless_Tracker_companion_radio_ble`) |
| **Heltec Wireless Tracker V2**<br>`variants/heltec_tracker_v2` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | 0.96-inch TFT, High-sensitivity GNSS, FEM/LNA | 9 envs (e.g. `heltec_tracker_v2_repeater`, `heltec_tracker_v2_repeater_bridge_espnow`) |

---

### Ikoka

*3 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Ikoka Handheld E22 (nRF52840)**<br>`variants/ikoka_handheld_nrf` | Nordic nRF52840 | E22 (SX1262 30dBm) | BLE, USB | High-power 1W PA, 0.96" OLED, Rotary Encoder | 7 envs (e.g. `ikoka_handheld_nrf_e22_30dbm_096_companion_radio_ble`, `ikoka_handheld_nrf_e22_30dbm_096_rotated_companion_radio_ble`) |
| **Ikoka Nano nRF52840**<br>`variants/ikoka_nano_nrf` | Nordic nRF52840 | SX1262 (22dBm) | BLE, USB | Ultra-miniature low-power node | 15 envs (e.g. `ikoka_nano_nrf_22dbm_companion_radio_usb`, `ikoka_nano_nrf_22dbm_companion_radio_ble`) |
| **Ikoka Stick nRF52840**<br>`variants/ikoka_stick_nrf` | Nordic nRF52840 | SX1262 (22dBm) | BLE, USB | Dongle / stick form factor, BLE companion | 15 envs (e.g. `ikoka_stick_nrf_22dbm_companion_radio_usb`, `ikoka_stick_nrf_22dbm_companion_radio_ble`) |

---

### KeepTeen

*1 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **KeepTeen LT1**<br>`variants/keepteen_lt1` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Color display, GPS, Rechargeable battery | 5 envs (e.g. `KeepteenLT1_repeater`, `KeepteenLT1_room_server`) |

---

### LilyGO

*14 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **LilyGO T-Beam (SX1262)**<br>`variants/lilygo_tbeam_SX1262` | ESP32 | SX1262 | Wi-Fi, BLE, USB | Neo-6M/M8N GPS, 18650 holder, AXP192/AXP2101 PMU | 5 envs (e.g. `Tbeam_SX1262_companion_radio_ble`, `Tbeam_SX1262_repeater`) |
| **LilyGO T-Beam (SX1276)**<br>`variants/lilygo_tbeam_SX1276` | ESP32 | SX1276 | Wi-Fi, BLE, USB | GPS, 18650 battery holder, AXP192 PMU | 5 envs (e.g. `Tbeam_SX1276_companion_radio_ble`, `Tbeam_SX1276_repeater`) |
| **LilyGO T-Beam 1W**<br>`variants/lilygo_tbeam_1w` | ESP32 | SX1262 (30dBm PA) | Wi-Fi, BLE, USB | 1W RF power amplifier, Active cooling fan, GPS/GNSS, 18650 holder | 7 envs (e.g. `LilyGo_TBeam_1W_repeater`, `LilyGo_TBeam_1W_room_server`) |
| **LilyGO T-Beam Supreme**<br>`variants/lilygo_tbeam_supreme_SX1262` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | U-blox MAX-M10Q GNSS, 0.96" OLED, Sensors, 18650 | 6 envs (e.g. `T_Beam_S3_Supreme_SX1262_repeater`, `T_Beam_S3_Supreme_SX1262_repeater_bridge_espnow`) |
| **LilyGO T-Deck**<br>`variants/lilygo_tdeck` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Full QWERTY Keyboard, 2.8-inch IPS Display, Trackball | `LilyGo_TDeck_companion_radio_usb`, `LilyGo_TDeck_companion_radio_ble`, `LilyGo_TDeck_repeater`, `LilyGo_TDeck_kiss_modem` |
| **LilyGO T-ETH Elite**<br>`variants/lilygo_teth_elite` | ESP32-S3 | SX1262 | Ethernet, Wi-Fi, BLE, USB | W5500 10/100M Ethernet port, PoE support, LoRa | 5 envs (e.g. `LilyGo_TETH_Elite_sx1262_repeater`, `LilyGo_TETH_Elite_sx1262_room_server`) |
| **LilyGO T-Echo**<br>`variants/lilygo_techo` | Nordic nRF52840 | SX1262 | BLE, USB | 1.54-inch E-Ink Display, GPS/GNSS, Touch button, LiPo | 5 envs (e.g. `LilyGo_T-Echo_repeater`, `LilyGo_T-Echo_room_server`) |
| **LilyGO T-Echo Card**<br>`variants/lilygo_techo_card` | Nordic nRF52840 | SX1262 | BLE, USB | Credit-card form factor, E-Ink, Ultra-thin LiPo | 5 envs (e.g. `LilyGo_T-Echo_Card_repeater`, `LilyGo_T-Echo_Card_room_server`) |
| **LilyGO T-Echo Lite**<br>`variants/lilygo_techo_lite` | Nordic nRF52840 | SX1262 | BLE, USB | E-Ink display, Reduced footprint without GPS | 6 envs (e.g. `LilyGo_T-Echo-Lite_repeater`, `LilyGo_T-Echo-Lite_room_server`) |
| **LilyGO T-Impulse Plus**<br>`variants/lilygo_t_impulse_plus` | Nordic nRF52840 | SX1262 | BLE, USB | Wristwatch form factor, OLED, RTC, IMU, Battery | `LilyGo_T_Impulse_Plus_repeater`, `LilyGo_T_Impulse_Plus_companion_radio_ble`, `LilyGo_T_Impulse_Plus_kiss_modem` |
| **LilyGO T-LoRa C6**<br>`variants/lilygo_tlora_c6` | ESP32-C6 | SX1262 | Wi-Fi 6, BLE 5, 802.15.4 | Compact next-gen RISC-V SoC | `LilyGo_Tlora_C6_repeater_`, `LilyGo_Tlora_C6_room_server_`, `LilyGo_Tlora_C6_companion_radio_ble_`, `LilyGo_Tlora_C6_kiss_modem` |
| **LilyGO T-LoRa V2.1-1.6**<br>`variants/lilygo_tlora_v2_1` | ESP32 | SX1276 | Wi-Fi, BLE, USB | 0.96-inch OLED, SMA antenna connector | 9 envs (e.g. `LilyGo_TLora_V2_1_1_6_repeater`, `LilyGo_TLora_V2_1_1_6_terminal_chat`) |
| **LilyGO T3-S3 (SX1262)**<br>`variants/lilygo_t3s3` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | 0.96-inch OLED, MicroSD card slot, LiPo | 7 envs (e.g. `LilyGo_T3S3_sx1262_repeater`, `LilyGo_T3S3_sx1262_repeater_bridge_espnow`) |
| **LilyGO T3-S3 (SX1276)**<br>`variants/lilygo_t3s3_sx1276` | ESP32-S3 | SX1276 | Wi-Fi, BLE, USB | SX1276 legacy support, MicroSD, LiPo | 7 envs (e.g. `LilyGo_T3S3_sx1276_repeater`, `LilyGo_T3S3_sx1276_repeater_bridge_espnow`) |

---

### M5Stack

*1 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **M5Stack Unit C6L**<br>`variants/m5stack_unit_c6l` | ESP32-C6 | SX1262 | Wi-Fi 6, BLE, USB | Grove compatible connector, Compact form factor | 5 envs (e.g. `M5Stack_Unit_C6L_repeater`, `M5Stack_Unit_C6L_room_server`) |

---

### Maker DIY

*4 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Mesh Adventurer**<br>`variants/meshadventurer` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Display, Navigation buttons, Rugged enclosure | 14 envs (e.g. `Meshadventurer_sx1262_repeater`, `Meshadventurer_sx1262_repeater_bridge_espnow`) |
| **MeshTiny**<br>`variants/meshtiny` | Nordic nRF52840 | SX1262 | BLE, USB | Minimalist coin-cell / tiny LiPo companion node | `Meshtiny_companion_radio_usb`, `Meshtiny_companion_radio_ble`, `Meshtiny_kiss_modem` |
| **Pro Micro nRF52840 DIY**<br>`variants/promicro` | Nordic nRF52840 | SX1262 | BLE, USB | Breadboard-friendly DIY build using Pro Micro nRF52 | 8 envs (e.g. `ProMicro_repeater`, `ProMicro_repeater_bridge_rs232_serial1`) |
| **Tiny Relay**<br>`variants/tiny_relay` | STM32WLE5JC | Integrated LoRa | USB Serial | Coin-sized ultra-low power mesh relay | `Tiny_Relay_repeater`, `Tiny_Relay_sensor`, `Tiny_Relay_companion_radio_usb`, `Tiny_Relay_kiss_modem` |

---

### Meshnology

*1 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Meshnology W12**<br>`variants/meshnology_w12` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Compact repeater and tracker node | 9 envs (e.g. `meshnology_w12_repeater`, `meshnology_w12_repeater_bridge_espnow`) |

---

### MinewSemi

*1 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **MinewSemi ME25LS01**<br>`variants/minewsemi_me25ls01` | Nordic nRF52840 | SX1262 | BLE, USB | Certified industrial LoRa + BLE module | 6 envs (e.g. `Minewsemi_me25ls01_companion_radio_ble`, `Minewsemi_me25ls01_repeater`) |

---

### Muzi Works

*2 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Muzi Base**<br>`variants/muzi_base` | Nordic nRF52840 | LR1121 (Duo) / SX1262 (Uno) | BLE, USB | Modular baseboard, SuperIO expansion, QSPI Flash, Advanced Power Mgt | 10 envs (e.g. `muzi_base_duo_repeater`, `muzi_base_uno_companion_radio_ble`) |
| **Muzi Works R1 Neo**<br>`variants/muziworks_r1_neo` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Field deployment repeater / room server enclosure | 7 envs (e.g. `R1Neo_repeater`, `R1Neo_room_server`) |

---

### Nibble

*2 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Nibble Screen Connect**<br>`variants/nibble_screen_connect` | ESP32-C6 | SX1262 | Wi-Fi 6, BLE, USB | Screen connector, Low-power routing | 8 envs (e.g. `nibble_screen_connect_repeater_`, `nibble_screen_connect_repeater_bridge_espnow_`) |
| **Nibble Zero Connect**<br>`variants/nibble_zero_connect` | ESP32-C6 | SX1262 | Wi-Fi 6, BLE, USB | Headless miniature mesh relay | 7 envs (e.g. `nibble_zero_connect_repeater_`, `nibble_zero_connect_repeater_bridge_espnow_`) |

---

### RAKwireless

*6 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **RAK WisMesh Tag**<br>`variants/rak_wismesh_tag` | Nordic nRF52840 | SX1262 | BLE, USB | Wearable tracker tag, Accelerometer, Buzzer, Battery | 6 envs (e.g. `RAK_WisMesh_Tag_repeater`, `RAK_WisMesh_Tag_room_server`) |
| **WisBlock RAK11310**<br>`variants/rak11310` | Raspberry Pi RP2040 | SX1262 | USB Serial | Dual-core RP2040 + WisBlock modular ecosystem | 6 envs (e.g. `RAK_11310_repeater`, `RAK_11310_repeater_bridge_rs232`) |
| **WisBlock RAK3112**<br>`variants/rak3112` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | Modular ESP32-S3 core for WisBlock bases | 10 envs (e.g. `RAK_3112_repeater`, `RAK_3112_repeater_bridge_rs232`) |
| **WisBlock RAK3172 / RAK3x72**<br>`variants/rak3x72` | STM32WLE5JC | Integrated LoRa (SX126x) | USB Serial | Ultra-low power Cortex-M4 + LoRa SoC | `RAK_3x72_repeater`, `RAK_3x72_sensor`, `RAK_3x72_companion_radio_usb`, `RAK_3x72_kiss_modem` |
| **WisBlock RAK3401**<br>`variants/rak3401` | ESP32-C6 | SX1262 | Wi-Fi 6, BLE, USB | WisBlock RISC-V wireless module | 7 envs (e.g. `RAK_3401_repeater`, `RAK_3401_room_server`) |
| **WisBlock RAK4631 / RAK4630**<br>`variants/rak4631` | Nordic nRF52840 | SX1262 | BLE, USB (Opt. Ethernet) | Supports OLED/E-Ink, GPS, Solar, RAK13800 Ethernet | 12 envs (e.g. `RAK_4631_repeater`, `RAK_4631_repeater_ethernet`) |

---

### Raspberry Pi

*1 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Raspberry Pi Pico W DIY**<br>`variants/rpi_picow` | Raspberry Pi RP2040 | SX1262 | Wi-Fi, USB | RP2040 + CYW43439 Wi-Fi paired with LoRa SPI module | 5 envs (e.g. `PicoW_repeater`, `PicoW_room_server`) |

---

### Seeed Studio

*16 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Seeed XIAO ESP32-C3**<br>`variants/xiao_c3` | ESP32-C3 | SX1262 (via expansion) | Wi-Fi, BLE, USB | Thumb-sized node, Expansion board with OLED | 6 envs (e.g. `Xiao_C3_repeater`, `Xiao_C3_room_server`) |
| **Seeed XIAO ESP32-C6 / Meshimi**<br>`variants/xiao_c6` | ESP32-C6 | SX1262 | Wi-Fi 6, BLE 5, USB | Thumb-sized node, Next-gen RISC-V core | 7 envs (e.g. `Xiao_C6_repeater_`, `Xiao_C6_companion_radio_ble_`) |
| **Seeed XIAO ESP32-S3**<br>`variants/xiao_s3` | ESP32-S3 | SX1262 (via expansion) | Wi-Fi, BLE, USB | Thumb-sized dual-core ESP32-S3 node | 7 envs (e.g. `Xiao_S3_repeater`, `Xiao_S3_repeater_bridge_espnow`) |
| **Seeed XIAO ESP32-S3 Wio**<br>`variants/xiao_s3_wio` | ESP32-S3 | SX1262 | Wi-Fi, BLE, USB | XIAO S3 paired with Wio LoRa expansion board | 10 envs (e.g. `Xiao_S3_WIO_repeater`, `Xiao_S3_WIO_repeater_bridge_espnow`) |
| **Seeed XIAO RP2040**<br>`variants/xiao_rp2040` | Raspberry Pi RP2040 | SX1262 (via expansion) | USB Serial | Thumb-sized dual-core RP2040 board | 5 envs (e.g. `Xiao_rp2040_repeater`, `Xiao_rp2040_room_server`) |
| **Seeed XIAO nRF52840**<br>`variants/xiao_nrf52` | Nordic nRF52840 | SX1262 (via expansion) | BLE, USB | Thumb-sized, Ultra-low power sleep, Battery sensing | 5 envs (e.g. `Xiao_nrf52_companion_radio_ble`, `Xiao_nrf52_companion_radio_usb`) |
| **SenseCAP Card Tracker T1000-E**<br>`variants/t1000-e` | Nordic nRF52840 | SX1262 | BLE, USB | Card form factor, GNSS/GPS, Sensors, Wireless charging | 5 envs (e.g. `t1000e_repeater`, `t1000e_room_server`) |
| **SenseCAP Indicator**<br>`variants/sensecap_indicator-espnow` | ESP32-S3 | ESP-NOW (No LoRa) | Wi-Fi, BLE, USB | 3.5-inch Touch Display, ESP-NOW network bridge | `SenseCapIndicator-ESPNow_comp_radio_usb` |
| **SenseCAP MeshTracker X1**<br>`variants/meshtracker_x1` | Nordic nRF52840 | SX1262 | BLE, USB | Rugged outdoor GNSS tracker, Waterproof, Solar | 5 envs (e.g. `MeshTracker_X1_repeater`, `MeshTracker_X1_room_server`) |
| **SenseCAP Solar Node**<br>`variants/sensecap_solar` | Nordic nRF52840 | SX1262 | BLE, USB | Integrated solar panel, MPPT charger, IP66 enclosure | 5 envs (e.g. `SenseCap_Solar_repeater`, `SenseCap_Solar_room_server`) |
| **Wio Tracker L1**<br>`variants/wio-tracker-l1` | Nordic nRF52840 | SX1262 | BLE, USB | GNSS/GPS, Accelerometer, LiPo charger, Grove | 5 envs (e.g. `WioTrackerL1_repeater`, `WioTrackerL1_room_server`) |
| **Wio Tracker L1 1W**<br>`variants/wio-tracker-l1-1w` | Nordic nRF52840 | SX1262 + 1W PA | BLE, USB | 1W high-power LoRa Front-End Module (FEM), GNSS/GPS, battery charger, Grove | 5 envs (e.g. `WioTrackerL1-1W_repeater`, `WioTrackerL1-1W_companion_radio_ble`) |
| **Wio Tracker L1 E-Ink**<br>`variants/wio-tracker-l1-eink` | Nordic nRF52840 | SX1262 | BLE, USB | E-Ink display, GPS, Rugged tracking | `WioTrackerL1Eink_companion_radio_ble`, `WioTrackerL1Eink_kiss_modem` |
| **Wio WM1110**<br>`variants/wio_wm1110` | Nordic nRF52840 | Semtech LR1110 | BLE, USB | Multi-constellation GNSS, Wi-Fi sniffing, LoRa | `wio_wm1110_repeater`, `wio_wm1110_room_server`, `wio_wm1110_companion_radio_ble`, `wio_wm1110_kiss_modem` |
| **Wio-E5 Development Board**<br>`variants/wio-e5-dev` | STM32WLE5JC | Integrated LoRa | USB Serial | Full breakout headers, Grove ports, Battery connector | `wio-e5_repeater`, `wio-e5-repeater_bridge_rs232`, `wio-e5_companion_radio_usb`, `wio-e5_kiss_modem` |
| **Wio-E5 Mini**<br>`variants/wio-e5-mini` | STM32WLE5JC | Integrated LoRa | USB Serial | Compact form factor dev board | `wio-e5-mini_repeater`, `wio-e5-mini_sensor`, `wio-e5-mini_companion_radio_usb`, `wio-e5-mini_kiss_modem` |

---

### Tenstar

*1 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Tenstar Robot C3**<br>`variants/tenstar_c3` | ESP32-C3 | SX1262 / SX1268 | Wi-Fi, BLE, USB | Cost-effective compact repeater node | 6 envs (e.g. `Tenstar_C3_sx1262_repeater`, `Tenstar_C3_sx1262_repeater_bridge_espnow`) |

---

### Waveshare

*2 supported targets*

| Board Name | MCU | LoRa Radio | Connectivity | Onboard Features | Target Environments |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Waveshare ESP32-S3-LR1121**<br>`variants/waveshare_esp32s3_lr1121` | ESP32-S3 | Semtech LR1121 | Wi-Fi, BLE, USB | Sub-GHz (850-930 MHz) + 2.4 GHz dual-band LoRa, 2MB PSRAM, native USB-CDC | 5 envs (e.g. `Waveshare_S3_LR1121_repeater`, `Waveshare_S3_LR1121_companion_radio_ble`) |
| **Waveshare RP2040-LoRa**<br>`variants/waveshare_rp2040_lora` | Raspberry Pi RP2040 | SX1262 | USB Serial | 1.14-inch LCD, Battery management, RP2040 | 6 envs (e.g. `waveshare_rp2040_lora_repeater`, `waveshare_rp2040_lora_repeater_bridge_rs232`) |

---

## 4. Architecture Quick Reference Matrix

| Feature | Nordic nRF52840 | ESP32 / ESP32-S3 | ESP32-C3 / C6 | RP2040 | STM32WLE5 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Sleep Current** | ~5-20 µA | ~1-5 mA (DCDC) | ~50-100 µA | ~1-3 mA | ~2-10 µA |
| **Bluetooth** | BLE 5.0 (Long Range) | BLE 4.2 / 5.0 | BLE 5.0 | None (Except Pico W) | None |
| **Wi-Fi** | No | 2.4 GHz 802.11 b/g/n | 802.11 b/g/n/ax (C6) | Pico W only | No |
| **Native USB** | Yes | Yes (S3 only) | Yes | Yes | Yes (with bootloader) |
| **Solar Repeater Suitability** | ⭐⭐⭐⭐⭐ (Exceptional) | ⭐⭐⭐ (Needs large solar panel) | ⭐⭐⭐⭐ (Good) | ⭐⭐⭐ (Moderate) | ⭐⭐⭐⭐⭐ (Exceptional) |
| **Mobile Companion Suitability** | ⭐⭐⭐⭐⭐ (Best battery life) | ⭐⭐⭐ (Higher drain) | ⭐⭐⭐⭐ (Good) | ⭐⭐ (Wired only) | ⭐⭐ (Wired only) |

---

## 5. Flashing and Building

### Web Flasher

The fastest way to install MeshCore firmware without compiling is using the official browser-based flasher:

👉 **[meshcore.io/flasher](https://meshcore.io/flasher)**


1. Connect your device via USB (using Chrome or Edge with Web Serial support).

2. Choose your device from the hardware dropdown.

3. Select the desired role (`Companion`, `Repeater`, or `Room Server`).

4. Click **Flash**.


### Web Configurator

Once flashed, configure node parameters (radio settings, name, ACL, regions) over USB:

👉 **[config.meshcore.io](https://config.meshcore.io)**


### Compiling from Source (PlatformIO)

To build any target environment locally using PlatformIO:

```bash
# List environments for a specific board
pio project data

# Build a specific target environment
pio run -e Heltec_v3_repeater

# Build and upload
pio run -e RAK_4631_repeater --target upload
```
