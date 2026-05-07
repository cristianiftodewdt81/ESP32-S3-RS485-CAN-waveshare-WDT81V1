# Flash Instructions - ESP32-S3 Waveshare CAN RS485

## Firmware Info
- **Chip**: ESP32-S3
- **Flash mode**: DIO
- **Flash freq**: 80MHz
- **Flash size**: 16MB
- **Compiled**: Feb 11 2026, IDF v5.5.2

---

## Method 1: WebFlash (browser, no tools needed)

Open `WebFlash/index.html` in Chrome/Edge and click "Install".

> `waveshare_full.bin` is a **merged flash image** flashed at offset `0x0`.  
> It contains: bootloader + partition table + firmware app.

---

## Method 2: esptool (manual, USB)

```bash
pip install esptool

esptool.py \
  --chip esp32s3 \
  --port /dev/ttyUSB0 \
  --baud 921600 \
  write_flash \
  --flash_mode dio \
  --flash_freq 80m \
  --flash_size 16MB \
  0x0 WebFlash/waveshare_full.bin
```

**Windows:**
```batch
esptool.py --chip esp32s3 --port COM3 --baud 921600 write_flash --flash_mode dio --flash_freq 80m --flash_size 16MB 0x0 WebFlash/waveshare_full.bin
```

> If the board is **bricked**: hold `BOOT`, press `RESET`, release `RESET`, release `BOOT`, then run esptool.

---

## Method 3: OTA (app binary only, board must be running)

Flash only `firmware/firmware.bin` at offset `0x10000` via OTA or:

```bash
esptool.py --chip esp32s3 --port /dev/ttyUSB0 --baud 921600 \
  write_flash 0x10000 firmware/firmware.bin
```

---

## File map

| File | Offset | Description |
|------|--------|-------------|
| `WebFlash/waveshare_full.bin` | `0x0` | **Merged image** (bootloader + partitions + app) |
| `firmware/firmware.bin` | `0x10000` | App binary only (for OTA / partial flash) |

