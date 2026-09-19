# Xiaozhi ESP32-C3 Custom – Cimahi

Custom **Xiaozhi AI** berbasis **ESP32-C3 Super Mini 4MB** dengan OLED SSD1306, microphone INMP441, amplifier MAX98357A, sensor DHT11, 2 relay untuk Lampu dan Kipas, serta tombol PTT.

Firmware menggunakan **Bahasa Indonesia** dan wake word **Hi, Jason**.

Project ini dibuat menggunakan source resmi Xiaozhi ESP32 dan ESP-IDF 6.1.

---

## Fitur

- ESP32-C3 Super Mini 4MB
- Bahasa Indonesia (`id-ID`)
- Wake Word: **Hi, Jason**
- OLED SSD1306 128×64
- Microphone INMP441
- Amplifier MAX98357A
- Sensor DHT11
- Pembacaan suhu
- Pembacaan kelembapan
- Relay Lampu
- Relay Kipas
- Tombol PTT / Push-to-Talk
- Startup branding OLED
- Relay aktif HIGH

Saat startup, OLED menampilkan:

```text
CREATED BY
ARSIPARIS
XIAOZHI CIMAHI
```

---

## Wake Word

Wake word yang digunakan:

**Hi, Jason**

Model ESP-SR:

```text
wn9s_hijason
```

Contoh:

> Hi, Jason

Kemudian:

> Nyalakan lampu

---

## Contoh Perintah Suara

### Lampu

```text
Nyalakan lampu
Matikan lampu
Lampu hidup
Lampu mati
Apakah lampu menyala?
```

### Kipas

```text
Nyalakan kipas
Matikan kipas
Kipas hidup
Kipas mati
Apakah kipas menyala?
```

### Suhu dan Kelembapan

```text
Berapa suhu sekarang?
Berapa kelembapannya?
Baca suhu dan kelembapan.
```

---

# Wiring

## 1. OLED SSD1306 128×64

| OLED | ESP32-C3 |
|---|---|
| GND | GND |
| VCC / VDD | 3.3V |
| SDA | GPIO8 |
| SCL / SCK | GPIO9 |

Alamat I2C:

```text
0x3C
```

---

## 2. DHT11

| DHT11 | ESP32-C3 |
|---|---|
| VCC | 3.3V |
| DATA | GPIO0 |
| GND | GND |

Jika menggunakan DHT11 sensor polos tanpa modul, gunakan resistor pull-up sekitar **10KΩ** antara DATA dan 3.3V.

---

## 3. Tombol PTT / Push-to-Talk

| Tombol | ESP32-C3 |
|---|---|
| Kaki 1 | GPIO3 |
| Kaki 2 | GND |

GPIO3 digunakan sebagai tombol bicara.

Tekan tombol PTT untuk berbicara dengan Xiaozhi.

---

## 4. INMP441

| INMP441 | ESP32-C3 |
|---|---|
| VDD | 3.3V |
| GND | GND |
| SCK | GPIO5 |
| WS | GPIO6 |
| SD | GPIO4 |
| L/R | GND |

Mapping:

```text
GPIO4 → INMP441 SD
GPIO5 → INMP441 SCK
GPIO6 → INMP441 WS
```

---

## 5. MAX98357A

| MAX98357A | ESP32-C3 |
|---|---|
| VIN | 5V |
| GND | GND |
| DIN | GPIO7 |
| BCLK | GPIO5 |
| LRC | GPIO6 |
| SPK+ | Speaker + |
| SPK- | Speaker - |

GPIO audio:

```text
GPIO5 → BCLK
GPIO6 → LRC
GPIO7 → DIN
```

GPIO5 dan GPIO6 digunakan bersama:

```text
GPIO5
 ├── INMP441 SCK
 └── MAX98357A BCLK

GPIO6
 ├── INMP441 WS
 └── MAX98357A LRC
```

---

## 6. Relay Lampu

| Relay Lampu | ESP32-C3 |
|---|---|
| IN | GPIO1 |
| GND | GND |
| VCC | Sesuai tegangan modul relay |

Logika:

```text
GPIO1 HIGH → Lampu ON
GPIO1 LOW  → Lampu OFF
```

---

## 7. Relay Kipas

| Relay Kipas | ESP32-C3 |
|---|---|
| IN | GPIO10 |
| GND | GND |
| VCC | Sesuai tegangan modul relay |

Logika:

```text
GPIO10 HIGH → Kipas ON
GPIO10 LOW  → Kipas OFF
```

---

# Peta GPIO Final

| GPIO | Fungsi |
|---|---|
| GPIO0 | DHT11 DATA |
| GPIO1 | Relay Lampu |
| GPIO2 | Tidak digunakan |
| GPIO3 | PTT / Tombol Bicara |
| GPIO4 | INMP441 SD |
| GPIO5 | I2S BCLK |
| GPIO6 | I2S WS / LRC |
| GPIO7 | MAX98357A DIN |
| GPIO8 | OLED SDA |
| GPIO9 | OLED SCL |
| GPIO10 | Relay Kipas |

---

# Power

```text
3.3V
 ├── OLED SSD1306
 ├── INMP441
 └── DHT11

5V
 └── MAX98357A

GND
 └── Semua GND disatukan
```

---

# Diagram Wiring

```text
                 ESP32-C3 SUPER MINI
                 ===================

GPIO0  ────────── DHT11 DATA

GPIO1  ────────── RELAY LAMPU

GPIO3  ────────── TOMBOL PTT
                    │
                   GND

GPIO4  ────────── INMP441 SD

GPIO5  ───────┬── INMP441 SCK
              └── MAX98357A BCLK

GPIO6  ───────┬── INMP441 WS
              └── MAX98357A LRC

GPIO7  ────────── MAX98357A DIN

GPIO8  ────────── OLED SDA

GPIO9  ────────── OLED SCL

GPIO10 ────────── RELAY KIPAS


3.3V ─────────── OLED
                 INMP441
                 DHT11

5V ───────────── MAX98357A

GND ──────────── SEMUA GND
```

---

# Startup OLED

Saat ESP32-C3 dinyalakan, OLED menampilkan:

```text
CREATED BY
ARSIPARIS
XIAOZHI CIMAHI
```

Setelah startup, perangkat masuk ke tampilan Xiaozhi.

---

# Build

Environment:

```text
ESP-IDF 6.1
ESP32-C3
Flash 4MB
```

Board:

```text
arsip-c3
```

Build:

```powershell
python scripts/build.py arsip-c3 --language id-ID --wake-word wn9s_hijason
```

---

# Firmware

Firmware final menggunakan merged binary.

Nama file:

```text
merged-binary.bin
```

Firmware ditujukan untuk:

```text
ESP32-C3
Flash 4MB
Language: id-ID
Wake Word: wn9s_hijason
```

---

# Flash Firmware

Contoh menggunakan esptool:

```cmd
python -m esptool --chip esp32c3 --port COM19 --baud 460800 write_flash --flash_size 4MB 0x000000 "ARSIP_C3_FINAL_ID_HIJASON_4MB.bin"
```

Sesuaikan `COM19` dengan COM port ESP32-C3.

---

# Backup Flash

Sebelum flashing firmware baru, disarankan melakukan backup seluruh flash 4MB:

```cmd
python -m esptool --chip esp32c3 --port COM19 read_flash 0x000000 0x400000 "BACKUP_ESP32C3.bin"
```

---

# Catatan Penting

GPIO sudah ditentukan oleh firmware. Jangan menukar pin dengan GPIO lain tanpa mengubah konfigurasi firmware.

Mapping final:

```text
GPIO0  → DHT11
GPIO1  → Relay Lampu
GPIO3  → PTT
GPIO4  → INMP441 SD
GPIO5  → BCLK
GPIO6  → WS / LRC
GPIO7  → MAX98357A DIN
GPIO8  → OLED SDA
GPIO9  → OLED SCL
GPIO10 → Relay Kipas
```

Untuk relay yang mengendalikan beban AC/PLN 220V, pemasangan bagian tegangan tinggi harus menggunakan isolasi dan pengamanan listrik yang sesuai. Jangan menghubungkan tegangan PLN langsung ke ESP32 atau breadboard.

---

# Project

**Arsiparis Xiaozhi Cimahi**

```text
ESP32-C3 Super Mini
        +
OLED SSD1306
        +
INMP441
        +
MAX98357A
        +
DHT11
        +
Relay Lampu
        +
Relay Kipas
        +
PTT
        +
Bahasa Indonesia
        +
Wake Word "Hi, Jason"
```

**Created by Arsiparis – Xiaozhi Cimahi**
