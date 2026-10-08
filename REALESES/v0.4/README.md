# ESP32-DOOM v0.4 firmware

Flash these images to an ESP32 with `esptool` using the corresponding offsets:

- `bootloader.bin` - `0x1000` (hex) / `4096` (decimal)
- `partition-table.bin` - `0x8000` (hex) / `32768` (decimal)
- `esp32-doom.bin` - `0x10000` (hex) / `65536` (decimal)

## Wi-Fi access point

Credentials configured for this v0.4 build:

- SSID: `DOOM_AP`
- Password: `12345678`

## Build and configure

Run these commands from the project root in a terminal where ESP-IDF is installed:

The project defaults target the original ESP32 with PSRAM enabled at 40 MHz and 16 MB flash, as configured in `sdkconfig.defaults`.

```bash
cd /workspaces/ESP32-DOOM-codespace
. /home/codespace/esp-idf/export.sh
idf.py set-target esp32
idf.py -D DOOM_STREAMING=ON -D DOOM_WIFI_AP=ON \
  -D WIFI_SSID="DOOM_AP" -D WIFI_PASSWORD="12345678" build
```

To customize the access point, replace `DOOM_AP` with the hotspot name and `12345678` with its password. For WPA2, use a password at least 8 characters long. Connect your phone or computer to that hotspot to access the ESP32 stream.

## WAD storage

No SD card is needed; this project reads the WAD from the ESP32’s built-in flash. You do need a DOOM WAD written separately at flash offset `0x210000`. The three release binaries don’t include the WAD itself.

The repo’s `upload_wad.ps1` mentions SPIFFS, but it uses a different upload flow; the firmware code currently reads the WAD directly from flash.

## LCD wiring (ST7789V)

Connect the display to the ESP32-WROVER board as follows:

| Display pin | ESP32 pin | Purpose |
|-------------|-----------|---------|
| GND | GND | Ground |
| VCC | 3.3V | Power |
| SCL | GPIO 18 | SPI clock |
| SDA | GPIO 23 | SPI MOSI |
| RES | GPIO 21 | Reset |
| DC | GPIO 22 | Data/command |
| CS | GPIO 5 | Chip select |
| BLK | 3.3V | Backlight |

The LCD works only with Wi-Fi streaming disabled. To build LCD-only firmware, use `-D DOOM_STREAMING=OFF -D DOOM_WIFI_AP=OFF` instead of the streaming build options above.

## Controls

| Key | Action |
|-----|--------|
| Up / W | Move forward |
| Down / S | Move backward |
| Left / A | Turn left |
| Right / D | Turn right |
| Ctrl | Fire |
| Space | Use / open |
| Shift | Run |
| Alt | Strafe |
| Tab | Automap |
| Escape | Menu |
| Enter | Select |
| 1-7 | Select weapon |
