# 🚗 Sistem Parkir Otomatis Berbasis RFID

<div align="center">

![Arduino](https://img.shields.io/badge/Arduino-00979D?style=for-the-badge&logo=Arduino&logoColor=white)
![C++](https://img.shields.io/badge/C%2B%2B-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)

**Sistem kontrol akses parkir otomatis dengan manajemen kapasitas real-time menggunakan teknologi RFID**

[Fitur](#-fitur) • [Teknologi](#-teknologi) • [Instalasi](#-cara-instalasi) • [Dokumentasi](#-dokumentasi)

</div>

---

## 📋 Deskripsi

Sistem parkir otomatis ini merupakan solusi IoT untuk mengelola area parkir dengan kontrol akses berbasis RFID dan monitoring kapasitas real-time. Sistem ini terdiri dari dua controller (Arduino Mega dan Arduino Uno) yang berkomunikasi secara serial untuk mengelola pintu masuk dan pintu keluar secara terpisah namun terintegrasi.

### Masalah yang Diselesaikan
- ✅ Kontrol akses parkir yang aman dan otomatis
- ✅ Monitoring kapasitas parkir secara real-time
- ✅ Pencegahan over-capacity dengan sistem lock otomatis
- ✅ Manajemen keluar-masuk kendaraan yang efisien
- ✅ Tracking jumlah kendaraan di area parkir

---

## ✨ Fitur

### 🎯 Fitur Utama
- **Kontrol Akses RFID**: Autentikasi kendaraan menggunakan kartu RFID
- **Manajemen Kapasitas**: Counter otomatis yang melacak jumlah kendaraan
- **Sistem Lock Otomatis**: Pintu masuk terkunci saat parkir penuh
- **Dual Controller**: Sistem terdistribusi dengan Mega (master) dan Uno (slave)
- **Real-time Monitoring**: Status parkir tersedia/penuh dengan indikator LED

### 🔧 Fitur Teknis
- **Auto Detection**: Sensor ultrasonik mendeteksi kendaraan lewat
- **Debounce Protection**: Mencegah pembacaan kartu ganda
- **Timeout Safety**: Proteksi motor dan sensor dengan timeout
- **Serial Communication**: Komunikasi antar Arduino menggunakan protokol custom
- **Counter Protection**: Counter tidak bisa minus atau melebihi kapasitas

### 🚨 Indikator Status
| Kondisi | LED Hijau | LED Merah | Buzzer |
|---------|-----------|-----------|--------|
| Parkir Tersedia | ON | OFF | - |
| Parkir Penuh | OFF | ON | - |
| Akses Diterima | ON | OFF | Beep 1x |
| Akses Ditolak | OFF | ON | Beep 3x |
| Parkir Penuh (tap kartu) | OFF | ON | Beep panjang |

---

## 🛠️ Teknologi

### Hardware
- **Mikrokontroler**
  - Arduino Mega 2560 (Master - Pintu Masuk)
  - Arduino Uno (Slave - Pintu Keluar)
- **Sensor & Aktuator**
  - MFRC522 RFID Reader (x2)
  - HC-SR04 Ultrasonic Sensor (x2)
  - BTS7960 Motor Driver (x2)
  - DC Motor (x2)
  - Limit Switch (x4)
  - LED (Hijau & Merah) (x2 set)
  - Buzzer (x2)

### Software
- **Bahasa**: C++ (Arduino)
- **IDE**: Arduino IDE
- **Library**:
  - `SPI.h` - Komunikasi RFID
  - `MFRC522.h` - Driver RFID reader

### Protokol Komunikasi
```
Uno → Mega:
- "CONFIRM_OUT\n"  : Notifikasi kendaraan keluar

Mega → Uno:
- "ACK\n"          : Konfirmasi counter updated
```

---

## 📦 Cara Instalasi

### Prasyarat
- Arduino IDE (versi 1.8.x atau lebih baru)
- Library MFRC522 (install via Library Manager)
- Kabel USB untuk programming
- Kartu RFID untuk testing

### Langkah Instalasi

#### 1. Clone Repository
```bash
git clone https://github.com/username/sistem-parkir-otomatis.git
cd sistem-parkir-otomatis
```

#### 2. Install Library
Buka Arduino IDE → Tools → Manage Libraries → Cari "MFRC522" → Install

#### 3. Konfigurasi UID Kartu RFID
Edit file `src/arduino_mega.ino` dan `src/arduino_uno.ino`:
```cpp
byte authorizedUID[][4] = {
  {0x19, 0x5A, 0xB5, 0x02},  // Ganti dengan UID kartu Anda
  {0xD2, 0xA2, 0x0F, 0x05}   // Tambahkan kartu lain jika perlu
};
```

**Cara mendapatkan UID kartu:**
- Upload sketch reader UID ke Arduino
- Tap kartu RFID pada reader
- Salin UID yang muncul di Serial Monitor

#### 4. Upload Code
**Arduino Mega (Pintu Masuk):**
1. Lepas koneksi TX/RX dari Uno (jika sudah terpasang)
2. Buka `src/arduino_mega.ino`
3. Pilih board: Arduino Mega 2560
4. Upload code
5. Pasang kembali koneksi TX/RX

**Arduino Uno (Pintu Keluar):**
1. Lepas koneksi TX/RX dari Mega (jika sudah terpasang)
2. Buka `src/arduino_uno.ino`
3. Pilih board: Arduino Uno
4. Upload code
5. Pasang kembali koneksi TX/RX

#### 5. Wiring
Lihat diagram lengkap di folder `hardware/wiring/`

**Koneksi Serial antar Arduino:**
```
Mega TX0 (pin 1) → Uno RX (pin 0)
Mega RX0 (pin 0) → Uno TX (pin 1)
Mega GND → Uno GND
```

#### 6. Konfigurasi Kapasitas
Edit di `arduino_mega.ino`:
```cpp
#define KAPASITAS_MAKSIMAL 5  // Ubah sesuai kebutuhan
```

---

## 📊 Hasil

### Demo Video
> Tambahkan link video demo di sini

### Screenshots
> Tambahkan screenshot/foto sistem di sini

### Serial Monitor Output
```
========================================
  SISTEM PARKIR OTOMATIS - MASTER
  PINTU MASUK + MANAJEMEN KAPASITAS
========================================
Kapasitas Maksimal: 5
----------------------------------------
Status Parkir: 0/5 (5 slot tersisa)
Status: TERSEDIA
----------------------------------------

Sistem siap. Menunggu kartu RFID...

UID Terdeteksi: 19 5A B5 02
>>> AKSES DITERIMA <<<
Membuka palang masuk...
Palang masuk terbuka.
Menunggu kendaraan masuk...
Kendaraan berhasil masuk!
Counter: 1/5
Menutup palang masuk...
Palang masuk tertutup.
```

### Performa Sistem
- **Response Time**: < 500ms (deteksi kartu hingga palang bergerak)
- **Sensor Range**: 50cm (dapat disesuaikan)
- **Motor Timeout**: 15 detik
- **Debounce Time**: 2 detik
- **Vehicle Detection**: 2 detik stabilitas setelah lewat

---

## 🚀 Pengembangan

### Roadmap
- [ ] LCD Display untuk menampilkan jumlah slot tersisa
- [ ] Web Dashboard untuk monitoring jarak jauh
- [ ] Integrasi dengan database untuk logging
- [ ] Mobile app untuk admin
- [ ] Sistem pembayaran otomatis
- [ ] Backup power menggunakan UPS
- [ ] Cloud integration (IoT Platform)

### Kontribusi
Kontribusi sangat diterima! Berikut cara berkontribusi:

1. Fork repository ini
2. Buat branch fitur (`git checkout -b feature/AmazingFeature`)
3. Commit perubahan (`git commit -m 'Add some AmazingFeature'`)
4. Push ke branch (`git push origin feature/AmazingFeature`)
5. Buat Pull Request

### Known Issues
- Serial Monitor tidak bisa digunakan bersamaan dengan komunikasi antar Arduino (gunakan LED untuk debugging)
- Motor harus dikalibrasi manual jika kecepatan tidak sesuai
- Sensor ultrasonik kadang membaca error pada jarak > 400cm (sudah ditangani dengan return 999)

---

## 📚 Dokumentasi

### Struktur Folder
```
project-name/
│
├── docs/
│   ├── diagram/          # Diagram sistem dan arsitektur
│   └── flowchart/        # Flowchart logika program
│
├── hardware/
│   └── wiring/           # Diagram wiring dan skematik
│
├── src/
│   ├── arduino_uno.ino   # Source code untuk Arduino Uno
│   └── arduino_mega.ino  # Source code untuk Arduino Mega
│
├── README.md             # Dokumentasi utama
└── LICENSE               # Lisensi project
```

### Pin Mapping

**Arduino Mega (Master):**
| Komponen | Pin | Deskripsi |
|----------|-----|-----------|
| RFID SS | 44 | SPI Slave Select |
| RFID RST | 49 | Reset RFID |
| Motor RPWM | 4 | PWM forward |
| Motor LPWM | 5 | PWM reverse |
| Motor R_EN | 6 | Enable forward |
| Motor L_EN | 7 | Enable reverse |
| Limit Atas | 27 | Limit switch atas |
| Limit Bawah | 29 | Limit switch bawah |
| LED Hijau | 35 | Indikator tersedia |
| LED Merah | 37 | Indikator penuh |
| Buzzer | 38 | Audio feedback |
| Ultrasonic Trig | 31 | Trigger sensor |
| Ultrasonic Echo | 33 | Echo sensor |
| Serial TX | 1 | Komunikasi ke Uno |
| Serial RX | 0 | Komunikasi dari Uno |

**Arduino Uno (Slave):**
| Komponen | Pin | Deskripsi |
|----------|-----|-----------|
| RFID SS | 10 | SPI Slave Select |
| RFID RST | 9 | Reset RFID |
| Motor RPWM | 5 | PWM forward |
| Motor LPWM | 6 | PWM reverse |
| Motor R_EN | 7 | Enable forward |
| Motor L_EN | 8 | Enable reverse |
| Limit Atas | A0 | Limit switch atas |
| Limit Bawah | A1 | Limit switch bawah |
| Ultrasonic Trig | 4 | Trigger sensor |
| Ultrasonic Echo | 3 | Echo sensor |
| Serial TX | 1 | Komunikasi ke Mega |
| Serial RX | 0 | Komunikasi dari Mega |

---

## 📄 Lisensi

Distributed under the MIT License. See `LICENSE` for more information.

```
MIT License

Copyright (c) 2024 [Your Name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 👥 Tim Pengembang

- **Your Name** - *Initial Work* - [GitHub Profile](https://github.com/yourusername)

---

## 🙏 Acknowledgments

- Arduino Community untuk support dan dokumentasi
- MFRC522 Library contributors
- Semua yang telah berkontribusi dalam project ini

---

## 📞 Kontak

Your Name - [@yourtwitter](https://twitter.com/yourtwitter) - email@example.com

Project Link: [https://github.com/yourusername/sistem-parkir-otomatis](https://github.com/yourusername/sistem-parkir-otomatis)

---

<div align="center">

**⭐ Jika project ini bermanfaat, berikan star di GitHub! ⭐**

Made with ❤️ using Arduino

</div>