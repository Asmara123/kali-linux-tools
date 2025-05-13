# FOREMOST

foremost adalah tool open-source berbasis command-line di Linux yang digunakan untuk merecovery file yang terhapus dari perangkat penyimpanan (hard disk, flashdisk, SD card, file image .img, dll). Awalnya dikembangkan oleh US Air Force Office of Special Investigations dan The Center for Information Systems Security Studies and Research (CISR) untuk keperluan digital forensik.

### Cara Kerja Foremost
Foremost bekerja dengan metode carving, yaitu:

> Mencari pola header dan footer dari file-file tertentu secara langsung dari data mentah perangkat.

Artinya :
- Tidak peduli apakah file tersebut ada di partisi aktif, rusak, atau terhapus dari file system.
- Foremost membaca sektor demi sektor dan mengekstrak file yang dikenali berdasarkan struktur biner.

### Format File yang Didukung

Secara default, foremost mendukung banyak jenis file, antara lain:

| Ekstensi | Tipe File        |
| -------- | ---------------- |
| jpg      | Gambar JPEG      |
| png      | Gambar PNG       |
| gif      | Gambar GIF       |
| pdf      | Dokumen PDF      |
| doc/xls  | Microsoft Office |
| zip      | File ZIP/RAR     |
| mp4      | Video            |
| wav      | Audio            |
| exe      | Binary Windows   |


### Sintaks Umum Perintah Foremost

```
sudo foremost -t all -v -i /dev/sdb -o hasil-recovery
```

### Penjelasan Opsi/Opsi Foremost

| Opsi | Arti                                                                |
| ---- | ------------------------------------------------------------------- |
| `-i` | Input: Perangkat (`/dev/sdX`) atau file image (`.img`, `.dd`, dll)  |
| `-o` | Output: Folder untuk menyimpan hasil recovery                       |
| `-t` | Tipe file yang ingin direcovery, misalnya: `jpg`, `pdf`, atau `all` |
| `-v` | Verbose mode: tampilkan proses secara detail di terminal            |
| `-q` | Quiet mode: tidak menampilkan proses                                |
| `-c` | Gunakan konfigurasi dari file `/etc/foremost.conf`                  |

### Contoh Penggunaan Foremost di Kali Linux

1. Instal terlebih dahulu (jika belum)

   ```
   sudo apt update
   sudo apt install foremost
   ```
2. Gunakan untuk recovery file dari SD card atau USB

   ```
   sudo foremost -i /dev/sdb -o hasil-recovery
   ```
3. Atau dari image file

   ```
   sudo foremost -i sdcard.img -o hasil-recovery
   ```

4. Filter hanya tipe file tertentu (misal hanya JPG)
   
   ```
   sudo foremost -t jpg -i /dev/sdb -o hasil-jpg
   ```

### Contoh Kasus Final

lsblk (list block devices) adalah perintah di Linux yang digunakan untuk melihat semua perangkat penyimpanan (seperti hard disk, SSD, USB, SD card) yang terhubung ke sistem, beserta partisinya.

📋 Fungsi Utama lsblk:

- Menampilkan daftar semua block device (penyimpanan)
- Mengetahui nama device seperti /dev/sda, /dev/sdb, dll
- Melihat ukuran, partisi, dan status mount (apakah sudah terpasang atau belum)

   ```
   lsblk
   ```
Karena foremost tidak bisa dijalankan ke device yang sedang terpasang (mounted), kamu perlu unmount dulu sda1:

1. Unmount terlebih dahulu:
   
   ```
   sudo umount /dev/sda1
   ```
> Jika ada pesan error (misalnya sedang dipakai), pastikan tidak sedang dibuka di file manager atau program lain.

2. Jalankan Foremost

   Setelah unmount, kamu bisa langsung gunakan foremost seperti ini:

   ```
   sudo foremost -t all -v -i /dev/sda -o hasil-recovery
   ```
   Penjelasan:
   - -t all = recovery semua tipe file (jpg, png, pdf, dll)
   - -v = tampilkan proses detail
   - -i /dev/sda = scan seluruh SD card (bukan hanya partisinya)
   - -o hasil-recovery = simpan hasil di folder hasil-recovery (nanti otomatis dibuat)
