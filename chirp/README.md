# CHIRP driver — UV-K5 (iu2vtm Custom + Messenger)

`iu2vtm.k5.chirp.v5.5.0.messenger.py` is the CHIRP driver for this K5
firmware (egzumer + F4HWN base). It is the **twin** of the K1 driver
(`iu2vtm.chirp.v5.5.0.messenger.py` in the K1 repo): same channel field
definitions and the same Messenger settings, so the two radios behave the
same way in CHIRP.

## Cross-compatibility with the K1

- **Channel copy/paste K1 ⇄ K5 works** because both drivers share identical
  channel definitions (tuning steps, modes FM/NFM/AM/NAM/USB, power levels,
  10-char names, channel encoding). No step incompatibilities.
- **The `.img` files are NOT interchangeable**: K1 and K5 use different
  memory chips (K1 = external SPI flash, map up to 0xB190; K5 = 8 KB I2C
  EEPROM, map 0x2000). To sync channels, open both images in CHIRP and
  copy/paste the memories — CHIRP converts them between the two models.
- **Encrypted messaging**: set the same `EncKey` password on both radios
  (same on-radio format, 10 chars padded with `_`) so they derive the same
  ChaCha20 key.

## Messenger settings (Basic Settings tab)

MsgRX, MsgACK, MsgMod (FSK 450 / FSK 700 / AFSK 1200), MsgEnc, EncKey —
identical to the K1 driver. K5 EEPROM addresses: MESSENGER_CONFIG at 0x0EA3
(byte 3 of the 0x0EA0 block), ENC_KEY at 0x0F30.

The driver class is `IU2VTMRadioK5` / model "UV-K5 (iu2vtm Custom)", renamed
from the base `UVK5RadioEgzumer` so it can coexist with the K1 driver in the
same CHIRP install without a class-name clash.

## Status

Built by adapting `uvk5_egzumer_f4hwn_ver_4_1_0.py` (same channel defs as the
K1 driver) plus the Messenger fields. Python-syntax checked. **Still needs a
load test in CHIRP and a download/upload round-trip on a real K5** to confirm
the EEPROM offsets read/write correctly.
