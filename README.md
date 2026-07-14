# soldeertraining-media-player

Fork of the original JC-Electronics soldeertraining.

ESPHome firmware for the soldeertraining board to turn it into a media player for Home Assistant. Currently only the `announcement_pipeline` works, because of a bug in ESPHome — this bug causes the mixer component to not work on the original ESP32.

This repo contains only the ESPHome firmware. For the PCB / schematic files (Eagle `.sch` / `.brd`) and the original board firmware, see the source repos below.

## Status

TTS / announcements only for now. Music playback (media_player) is not working yet due to the bug described below. More firmware will be added here as it's finished.

## Known issues

- ESPHome regression (introduced in 2026.4.x, removal of the legacy I2S driver) causes an `i2s_channel_disable: the channel has not been enabled yet` error on the standard ESP32 when the mixer component is used. The mixer is required for running a media pipeline alongside the announcement pipeline, so this currently blocks music playback on this hardware.
- Fix planned: move to an ESP32-S3 with PSRAM (ESP32-S3-DevKitC-1-N8R8), wired alongside the existing PCB. Not a drop-in replacement.

## Hardware

- ESP32-WROOM-32
- PCM5122 DAC (I2C address 0x4C, hardwired into I2C mode — requires explicit I2C init and `audio_dac.mute_off` to leave standby)
- TPA3130 amplifier

## Pinout

I2S uses the following pins: ESP32 pin - I2S signal

| ESP32 pin | I2S signal |
|-----------|------------|
| GPIO25    | LRCK       |
| GPIO27    | BCLK       |
| GPIO26    | DATA       |

I2C uses the following pins: ESP32 pin - I2C signal

| ESP32 pin | I2C signal |
|-----------|------------|
| GPIO32    | SCL        |
| GPIO33    | SDA        |

AMP enable is on GPIO18. Pulled to GND by default (100kΩ), must be driven HIGH in software to enable the amplifier.

## Boot sequence

To avoid an audio pop on boot:

1. `audio_dac.mute_off`
2. wait 200ms
3. turn on `amp_enable` (GPIO18)

## Configuration

`ESPHome_announcement_test_firmware.yaml` — confirmed working TTS/announcement-only config, pulled straight from ESPHome. API key moved to `!secret api_key`.

## Source / hardware

- Original firmware: [RnD-JC-Electronics/Soldeertraining](https://github.com/RnD-JC-Electronics/Soldeertraining) — contains the PCB schematic/board files (`.sch` / `.brd`) and the original (non-ESPHome) firmware.
- That repo is itself based on [MrBuddyCasino/ESP32_MP3_Decoder](https://github.com/MrBuddyCasino/ESP32_MP3_Decoder).
