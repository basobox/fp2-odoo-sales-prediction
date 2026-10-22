# Tutorial Git dan GitHub di Termux Android

**Tujuan:** Menggunakan Git di Termux untuk mengelola versi kode dan menyinkronkan proyek Python ke GitHub.

- **Konteks proyek:** FP2 — Odoo Sales Prediction
- **Perangkat:** HP atau tablet Android
- **Editor:** Helix, Nano, atau editor terminal lainnya
- **Remote repository:** GitHub
- **Metode autentikasi:** SSH

---

## 1. Memahami Git dan GitHub

Git adalah sistem version control untuk mencatat perubahan file dan kode. GitHub adalah layanan online untuk menyimpan repository Git dan membagikan proyek.

Alur kerja:

1. Menulis atau mengubah kode di Termux.
2. Memeriksa perubahan dengan Git.
3. Menyimpan perubahan menggunakan commit.
4. Mengirim perubahan ke GitHub menggunakan push.
5. Mengambil perubahan dari GitHub menggunakan pull.

### Istilah Penting

| Istilah | Fungsi |
|---|---|
| Repository | Folder proyek yang dikelola Git |
| Working directory | Area tempat mengedit file |
| Staging area | Area untuk memilih perubahan yang akan di-commit |
| Commit | Rekaman perubahan pada repository |
| Remote | Repository Git di lokasi lain, misalnya GitHub |
| Push | Mengirim commit lokal ke remote |
| Pull | Mengambil dan mengintegrasikan perubahan remote |
| Clone | Menyalin repository remote ke perangkat lokal |
| Branch | Jalur pengembangan kode yang terpisah |

---

## 2. Memeriksa Instalasi Git dan SSH

Jalankan di Termux:

```bash
git --version
ssh -V
```

Jika Git belum terpasang:

```bash
pkg update
pkg install git openssh
```

Untuk memperbarui paket yang sudah terpasang:

```bash
pkg update && pkg upgrade
```

Perintah pembaruan ini terutama disarankan saat menyiapkan lingkungan Termux atau ketika dibutuhkan, bukan setiap kali akan melakukan push.

## 3. Mengatur Identitas Git

Nama dan email ini akan tercatat pada commit.

```bash
git config --global user.name "Nama Kamu"
git config --global user.email "email@example.com"
```

Ganti contoh di atas dengan nama dan email yang ingin digunakan.

Periksa konfigurasi:

```bash
git config --global --list
```

Catatan: email commit tidak harus ditampilkan secara publik di GitHub. Jika ingin menjaga privasi, kamu dapat menggunakan alamat email `noreply` dari pengaturan GitHub.

## 4. Membuat SSH Key

SSH memungkinkan Termux terhubung ke GitHub tanpa memasukkan password akun setiap kali melakukan push.

Buat SSH key:

```bash
ssh-keygen -t ed25519 -C "email@example.com"
```

Ganti email dengan email yang digunakan untuk GitHub.

Ketika muncul pertanyaan lokasi penyimpanan key, tekan `Enter` untuk menggunakan lokasi default.

Jika ditanya passphrase, kamu dapat membuat passphrase untuk keamanan tambahan. Jika ingin tanpa passphrase, tekan `Enter` pada kolom tersebut.

### Menampilkan Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

Salin seluruh isi public key yang ditampilkan.

Penting:

- `~/.ssh/id_ed25519.pub` adalah public key yang boleh didaftarkan ke GitHub.
- `~/.ssh/id_ed25519` adalah private key dan tidak boleh dibagikan.
- Jangan mengunggah private key ke repository.

Dokumentasi resmi: [Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

## 5. Mendaftarkan SSH Key di GitHub

- Buka [GitHub SSH Keys](https://github.com/settings/keys).
- Login ke akun GitHub.
- Pilih **New SSH key**.
- Isi Title, misalnya `Termux Android`.
- Pilih Key type `Authentication Key`.
- Tempel public key dari Termux.
- Pilih **Add SSH key**.

### Menguji Koneksi SSH

Jalankan:

```bash
ssh -T git@github.com
```

Pada koneksi pertama, SSH mungkin meminta konfirmasi host.

Sebelum menyetujui, periksa fingerprint host berdasarkan dokumentasi resmi GitHub:

[GitHub SSH Key Fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)

Jika autentikasi berhasil, GitHub akan menampilkan pesan sambutan yang menyebut username akun.

Pesan bahwa GitHub tidak menyediakan shell access adalah normal.

Jika gagal, periksa apakah public key sudah terdaftar dan apakah private key tersedia di lokasi yang benar.

## 6. Membuat Repository di GitHub

Buka [GitHub — New Repository](https://github.com/new).

Gunakan konfigurasi berikut:

- **Repository name:** `fp2-odoo-sales-prediction`
- **Description:** Proyek Python, analisis data, dan prediksi penjualan berbasis ERP Odoo.
- **Visibility:** Public untuk portofolio yang dapat dilihat recruiter atau Private jika belum ingin membagikannya.

Untuk prosedur pemula di bawah ini, jangan centang opsi pembuatan README, `.gitignore`, atau license saat membuat repository.

Setelah repository dibuat, salin alamat SSH-nya. Bentuk alamat:

```text
git@github.com:USERNAME/fp2-odoo-sales-prediction.git
```

Ganti `USERNAME` dengan username GitHub milikmu.

## 7. Membuat Folder Kerja dan Melakukan Clone

Buat folder khusus proyek:

```bash
mkdir -p ~/projects
cd ~/projects
```

Clone repository:

```bash
git clone git@github.com:USERNAME/fp2-odoo-sales-prediction.git
```

Masuk ke folder proyek:

```bash
cd fp2-odoo-sales-prediction
```

Periksa status repository:

```bash
git status
```

Periksa alamat remote:

```bash
git remote -v
```

Hasil remote seharusnya menampilkan alamat repository GitHub milikmu.

## 8. Membuat README Pertama

Buat file README:

```bash
nano README.md
```

Isi file dengan:

```markdown
# FP2 - Odoo Sales Prediction

## Tujuan

Menganalisis data penjualan dan membangun model prediksi
untuk mendukung pengambilan keputusan bisnis.

## Teknologi

- Python
- Pandas
- SQL
- Machine Learning
- Odoo

## Status Proyek

Tahap pembelajaran Python dan persiapan data.
```

Simpan file di Nano:

- Tekan `Ctrl + O`.
- Tekan `Enter` untuk mengonfirmasi nama file.
- Tekan `Ctrl + X` untuk keluar.

Jika menggunakan Helix, buka file dengan:

```bash
hx README.md
```

Simpan perubahan di Helix dengan `:w`, lalu keluar dengan `:q`.

## 9. Siklus Git Pertama: Add, Commit, Push

### 9.1 Memeriksa Perubahan

```bash
git status
```

### 9.2 Memasukkan File ke Staging Area

```bash
git add README.md
```

Untuk menambahkan semua perubahan di folder proyek:

```bash
git add .
```

Periksa kembali sebelum commit:

```bash
git status
git diff --cached
```

Pastikan tidak ada file rahasia, token, password, atau data pribadi yang tidak sengaja ikut ditambahkan.

### 9.3 Membuat Commit

```bash
git commit -m "Add project README"
```

Commit adalah rekaman perubahan di repository lokal.

### 9.4 Mengirim Commit ke GitHub

```bash
git push -u origin main
```

Perintah tersebut mengirim commit ke branch `main` dan mengatur upstream agar push berikutnya lebih sederhana.

Jika branch utama bukan `main`, periksa nama branch:

```bash
git branch --show-current
```

Setelah push berhasil, buka repository di GitHub dan pastikan `README.md` sudah tampil.

## 10. Alur Kerja Sehari-hari

### 10.1 Sebelum Mulai Bekerja

Ambil perubahan terbaru dari GitHub:

```bash
git pull --rebase
```

Ini penting terutama jika repository juga diubah dari laptop, Google Colab, atau perangkat lain.

Jika branch lokal belum memiliki upstream, gunakan:

```bash
git pull --rebase origin main
```

Sesuaikan `main` dengan branch yang digunakan.

### 10.2 Setelah Mengedit File

Periksa perubahan:

```bash
git status
git diff
```

Tambahkan file yang akan disimpan:

```bash
git add .
```

Buat commit:

```bash
git commit -m "Add Python fundamentals exercises"
```

Kirim ke GitHub:

```bash
git push
```

### Ringkasan Alur Kerja

```text
Mulai bekerja
     |
     v
git pull --rebase
     |
     v
Edit atau buat file
     |
     v
git status
     |
     v
git add .
     |
     v
git commit -m "Pesan perubahan"
     |
     v
git push
```

Catatan: jika `git pull --rebase` berhenti karena konflik, selesaikan konflik terlebih dahulu. Jangan melakukan push sebelum masalah integrasi selesai.

## 11. Memahami `git pull` dan Konflik

Masalah sering terjadi ketika file lokal dan file di GitHub berubah secara terpisah.

Contoh:

- README di GitHub diubah melalui browser.
- README lokal diubah melalui Termux.
- Commit lokal belum mencakup perubahan remote.

Sebelum push, jalankan:

```bash
git pull --rebase
```

Jika muncul konflik:

```bash
git status
```

Buka file yang dilaporkan Git dan cari penanda konflik:

```text
<<<<<<<
=======
>>>>>>>
```

Edit file untuk mempertahankan isi yang benar dan hapus penanda konflik.

Setelah konflik diselesaikan:

```bash
git add README.md
git rebase --continue
```

Ulangi proses jika masih ada konflik lain. Setelah rebase selesai, periksa kembali:

```bash
git status
git log --oneline -5
```

Kemudian:

```bash
git push
```

Jika belum yakin cara menyelesaikan konflik, berhenti dan periksa pesan error terlebih dahulu. Jangan langsung menggunakan `git reset --hard` atau memaksa push karena tindakan tersebut dapat menghilangkan perubahan.

## 12. Struktur Repository FP2

Struktur proyek dapat dikembangkan secara bertahap menjadi:

```text
fp2-odoo-sales-prediction/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 00_project_setup/
│   ├── 01_python_fundamentals/
│   ├── 02_data_processing/
│   ├── 03_eda/
│   ├── 04_machine_learning/
│   ├── 05_erp_bi_integration/
│   └── 06_final_report/
├── src/
├── reports/
└── site/
```

Fungsi folder:

- `notebooks/`: latihan dan eksperimen Python, EDA, dan Machine Learning.
- `src/`: kode Python yang digunakan ulang.
- `reports/`: laporan analisis dan hasil proyek.
- `site/`: dokumentasi atau hasil publikasi, jika diperlukan.
- `requirements.txt`: daftar dependensi Python proyek.
- `README.md`: penjelasan proyek dan cara menjalankannya.

Buat folder saat diperlukan agar repository tidak dipenuhi folder kosong.

## 13. Perintah Git yang Perlu Dikuasai

| Perintah | Kegunaan |
|---|---|
| `git status` | Memeriksa status file |
| `git diff` | Melihat perubahan yang belum di-stage |
| `git diff --cached` | Melihat perubahan yang sudah di-stage |
| `git add FILE` | Memasukkan file tertentu ke staging |
| `git add .` | Memasukkan perubahan di folder kerja ke staging |
| `git commit -m "pesan"` | Merekam perubahan |
| `git log --oneline -5` | Melihat lima commit terakhir |
| `git pull --rebase` | Mengambil dan mengintegrasikan perubahan remote |
| `git push` | Mengirim commit ke remote |
| `git branch --show-current` | Menampilkan branch aktif |
| `git remote -v` | Memeriksa alamat remote |
| `git clone URL` | Menyalin repository remote |

## 14. Checklist Keberhasilan

- [ ] Git dan OpenSSH terpasang di Termux.
- [ ] Identitas Git telah dikonfigurasi.
- [ ] SSH key dibuat.
- [ ] Public key terdaftar di GitHub.
- [ ] Tes `ssh -T git@github.com` berhasil.
- [ ] Repository FP2 dibuat di GitHub.
- [ ] Repository berhasil di-clone ke Termux.
- [ ] README berhasil dibuat.
- [ ] Commit pertama berhasil.
- [ ] Push berhasil dan perubahan tampil di GitHub.
- [ ] Memahami kapan menggunakan `git pull --rebase`.

## 15. Referensi Resmi

- [Git Documentation](https://git-scm.com/docs)
- [Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [GitHub SSH Key Fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)
- [GitHub Repositories](https://docs.github.com/en/repositories)

## Target Akhir

Termux menjadi lingkungan kerja Python di Android, Git mengelola riwayat kode, dan GitHub menyimpan repository FP2 sebagai dokumentasi pembelajaran serta portofolio proyek.
