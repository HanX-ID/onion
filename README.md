## Panduan membuat web .onion pribadi dengan Tor

Panduan lengkap untuk membuat dan menjalankan website `.onion` pribadi menggunakan Tor Hidden Service. Cocok dijalankan di Termux (Android) atau di server Linux/VPS.

---

### Persyaratan

- Koneksi internet
- Termux (Android) atau Linux/VPS
- Tor Browser (untuk mengakses web onion)

---

### Langkah-langkah

#### 1. Update & Install Tor

**Untuk pengguna Termux:**

```bash
pkg update && pkg upgrade
pkg install tor
```

**Untuk pengguna Debian/Ubuntu:**

```bash
sudo apt update && sudo apt upgrade
sudo apt install tor
```

---

#### 2. Buat Folder untuk Website

```bash
mkdir -p ~/web
cp index.html ~/web/
```

> Siapkan file `index.html` terlebih dahulu di direktori yang sama sebelum menjalankan perintah di atas.

---

#### 3. Jalankan Web Server Lokal (Port 8080)

```bash
cd ~/web
python -m http.server 8080
```

---

#### 4. Buat File Konfigurasi Tor

Buat file bernama `torrc`:

```bash
nano torrc
```

Isi dengan:

```ini
HiddenServiceDir /data/data/com.termux/files/home/hidden_service/
HiddenServicePort 80 127.0.0.1:8080
```

> Jika kamu menggunakan Linux (bukan Termux), ubah path `HiddenServiceDir` sesuai direktori home kamu, misalnya:
> `/home/namauser/hidden_service/`

---

#### 5. Jalankan Tor

```bash
tor -f torrc
```

Tor akan otomatis membuat folder `hidden_service` dan menghasilkan domain `.onion`.

---

#### 6. Ambil Domain .onion

Setelah Tor berjalan, jalankan:

```bash
cat ~/hidden_service/hostname
```

Contoh hasil output:

![contoh hostname output](https://files.catbox.moe/ki8idj.jpg)

Hasilnya adalah domain `.onion` yang bisa kamu akses via Tor Browser.

---

#### 7. Akses Website

Buka **Tor Browser**, lalu masukkan alamat `.onion` yang dihasilkan.  
Website kamu sekarang bisa diakses secara anonim melalui jaringan Tor.

---

### Catatan Tambahan

- Port default Hidden Service adalah 80, bisa diganti sesuai kebutuhan.
- File domain dan private key ada di folder `hidden_service`. Jangan dibagikan jika ingin domain tetap pribadi.
- Website ini hanya bisa diakses melalui **Tor Browser**.

---

### Troubleshooting

- Jika muncul error `dpkg was interrupted`, jalankan:

```bash
dpkg --configure -a
```

- Jika menggunakan Termux dan muncul error permission, install `proot`:

```bash
pkg install proot
proot -0 dpkg --configure -a
```

- Jika website tidak bisa diakses, pastikan:
  - Port 8080 aktif
  - File `index.html` tersedia
  - Tor sedang berjalan
