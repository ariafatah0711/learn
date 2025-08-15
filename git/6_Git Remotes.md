# Git Remotes
## 1. Our Command to create remotes
#### Apa itu Remote Repository?
Remote repository hanyalah salinan (copy) dari repository kita yang disimpan di komputer lain (biasanya server atau cloud).
Kita bisa berinteraksi dengannya lewat internet untuk mengirim (push) atau mengambil (pull) commit.

Kelebihan Remote Repository
- Backup cadangan: Jika data lokal hilang, kita bisa memulihkan dari remote.
- Kolaborasi: Teman atau rekan kerja bisa dengan mudah mengunduh dan mengirim perubahan.
- Integrasi dengan layanan visualisasi: Seperti GitHub atau GitLab yang memudahkan memantau aktivitas project.

> Intinya: Remote repository adalah tulang punggung kerja sama di Git.

#### Perbedaan Lokal dan Remote
Sebelum ini, kita fokus di local repository (branching, merging, rebasing).
Sekarang, kita masuk ke dunia remote repository untuk menghubungkan lokal dengan repositori lain.

### visual
![alt text](<images/6_Git Remotes/image.png>)
```bash
git clone
```
![alt text](<images/6_Git Remotes/image-1.png>)

---

## 2. Git Remote Branches
#### Apa yang Berubah Setelah git clone?
Saat kamu menjalankan git clone (di simulasi ini), akan muncul branch baru di local repository, misalnya o/main.
Branch ini disebut remote branch.

#### Apa Itu Remote Branch?
- Fungsi utama: Menunjukkan status terakhir dari branch yang ada di remote repository (sejak terakhir kali kita berkomunikasi dengan remote).
- Manfaat: Membantu kita membedakan mana pekerjaan lokal dan mana yang sudah dipublikasikan.
- Lokasinya: Remote branch sebenarnya tersimpan di local repository, bukan di remote.

#### Kenapa Tidak Bisa Edit Remote Branch Langsung?
Kalau kita checkout remote branch (misalnya o/main), Git akan masuk ke detached HEAD mode.
Artinya:
- Kita tidak bisa langsung commit di branch itu.
- Commit yang dibuat di mode ini tidak akan memengaruhi remote branch.
- Remote branch (o/main) hanya akan berubah jika remote repository berubah (misalnya setelah kita push/pull).

> Di dunia nyata, nama remote biasanya origin (Git otomatis menamainya begitu saat git clone). Di visualisasi ini, o adalah singkatan dari origin.

### visual
```bash
git commit       # Commit baru di main (lokal)
git checkout C1  # Checkout commit lama / remote branch → masuk detached HEAD mode
git commit       # Commit baru di detached HEAD (tidak memengaruhi o/main)
```

---

## 3. Git Fetch
#### Fungsi Dasar
Bekerja dengan remote repository pada dasarnya adalah mengirim dan mengambil data dari repository lain.
git fetch digunakan untuk mengambil commit terbaru dari remote tanpa mengubah branch lokal kita.

#### Apa yang Dilakukan git fetch?
Perintah ini melakukan dua hal utama:
- Mengunduh commit baru yang ada di remote tapi belum ada di lokal.
- Memperbarui remote branch (misalnya o/main) agar sesuai dengan keadaan terbaru di remote.
> Intinya: git fetch menyinkronkan representasi lokal dari remote repository dengan kondisi terkini remote.

#### Apa yang Tidak Dilakukan git fetch?
- Tidak mengubah branch lokal (main tetap di posisi terakhir kita).
- Tidak memengaruhi file di working directory.
- Hanya memperbarui informasi remote branch di lokal.

> Banyak yang salah paham mengira git fetch langsung membuat lokal sama seperti remote. Padahal, dia hanya download data, belum menggabungkannya ke lokal.

Misalnya:


### visual
Remote punya commit C4, C5, C6, dan C7 yang belum ada di lokal.
![alt text](<images/6_Git Remotes/image-2.png>)

```bash
git fetch
```

![alt text](<images/6_Git Remotes/image-3.png>)

---

## 4. Git Pull
