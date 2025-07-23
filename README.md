# Deploy Aplikasi Flask

Project Flask minimalis yang gampang banget dipakai buat bikin aplikasi web simpel dan ringan. Semua kodenya cuma di satu file (`app.py`), plus ada template HTML dan file statis dasar biar gampang dikembangin.

## Fitur

* Aplikasi Flask cuma satu file (`app.py`), jadi gak ribet dan cepet paham.
* Struktur template HTML sederhana, ada sedikit styling dan JavaScript.
* Setup gampang tanpa hal-hal yang nggak perlu.
* Bisa kamu modifikasi dengan cepat buat bikin aplikasi web sendiri.

## Cara Instalasi dan Setup di AWS
1. Fork repo flask-minimal paknux ke akun GitHub kamu.

2. Buat instance Ubuntu di AWS EC2:

   * Pilih Amazon Machine Image (AMI) Ubuntu terbaru.

   * Saat setup Security Group, tambahkan aturan inbound rule untuk port 5000 dengan sumber 0.0.0.0/0 supaya aplikasi bisa diakses dari mana saja.

   * Jangan lupa juga tambahkan port 22 untuk SSH akses.

3. Connect ke instance Ubuntu
   
## Persiapan

Jalankan perintah ini dulu buat update dan install Python beserta toolsnya:

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip -y
```

## Cara Instalasi

1. Clone project ini ke komputer kamu:

   ```bash
   git clone https://github.com/yourusername/flask-minimal.git
   cd flask-minimal
   ```

2. Buat virtual environment (biar bersih dan rapi):

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. Install semua package yang dibutuhkan:

   ```bash
   pip install -r requirements.txt
   ```

4. Jalankan aplikasinya:

   ```bash
   python app.py
   ```

Buka browser kamu dan akses [http://localhost:5000](http://localhost:5000) buat lihat aplikasinya.

## Menjalankan Flask Otomatis dengan systemd

Supaya aplikasi Flask kamu jalan terus di background dan otomatis start saat instance di-reboot, ikuti langkah ini:

1. Buat file service systemd baru:

```bash
sudo nano /etc/systemd/system/flask-minimal.service
```

2. Isi file `flask-minimal.service` dengan konfigurasi berikut (sesuaikan path dan user):

```ini
[Unit]
Description=Flask Minimal App
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/flask-minimal
Environment="PATH=/home/ubuntu/flask-minimal/venv/bin"
ExecStart=/home/ubuntu/flask-minimal/venv/bin/python app.py

Restart=always

[Install]
WantedBy=multi-user.target
```

> **Catatan:**
>
> * Ganti `ubuntu` dengan username kamu di instance AWS.
> * Pastikan path `WorkingDirectory` dan `ExecStart` sesuai lokasi project dan virtual environment kamu.

3. Simpan dan keluar dari editor (`Ctrl+O`, `Enter`, lalu `Ctrl+X` di nano).

4. Reload systemd supaya mengenali service baru:

```bash
sudo systemctl daemon-reload
```

5. Aktifkan service supaya jalan otomatis saat boot:

```bash
sudo systemctl enable flask-minimal.service
```

6. Jalankan service sekarang juga:

```bash
sudo systemctl start flask-minimal.service
```

7. Cek status service untuk memastikan berjalan:

```bash
sudo systemctl status flask-minimal.service
```

Jika semua benar, aplikasi Flask kamu sekarang berjalan otomatis di port 5000 dan akan tetap jalan walau kamu logout atau instance reboot.
