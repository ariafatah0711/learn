# Introduction Sequence
## 1. Git Commits
Commit di Git adalah snapshot dari semua file yang sedang dilacak dalam proyek kita.

Bayangin kayak foto polaroid proyek kamu di satu momen tertentu. Tapi Git itu pintar

dia nggak nyimpen foto utuh setiap kali commit, melainkan cuma nyimpen perubahan yang terjadi sejak commit terakhir.

### visual
Misalnya kita mulai dengan commit awal: C0.
Lalu kita membuat perubahan dan commit lagi, jadilah C1.

![alt text](<images/1_Introduction Sequence/image.png>)

Sekarang kita jalankan perintah:
```bash
git commit
```

Maka Git akan membuat commit baru, misalnya C2:

![alt text](<images/1_Introduction Sequence/image-1.png>)

## 2. Git Branches
Branch di Git itu seperti cabang pohon.
Batang pohon = branch utama (main), dan commit adalah simpul di batang itu.

Kalau kita bikin branch baru, itu seperti menumbuhkan cabang dari titik tertentu di batang. Cabang itu akan punya riwayatnya sendiri, tapi tetap tersambung ke batang di titik awalnya.

Karena membuat cabang itu ringan (nggak makan banyak “nutrisi” alias penyimpanan), banyak orang pakai prinsip:

🌿 Cabang lebih awal, cabang lebih sering.

### visual
Misalnya, kita punya satu branch utama:
![alt text](<images/1_Introduction Sequence/image-2.png>)

Lalu kita ingin membuat branch baru bernama bugFix:
```bash
git branch bugFix
```

![alt text](<images/1_Introduction Sequence/image-3.png>)

Sekarang, bugFix hanya menunjuk ke commit yang sama dengan branch main saat ini:

Tapi, kalau kita ingin mulai bekerja di branch baru itu, kita harus berpindah ke branch tersebut:

```bash
git checkout bugFix
```

## 3. Branches and Merging
Kita sudah tahu cara membuat commit dan branch.
Sekarang, gimana caranya menggabungkan hasil kerja dari dua branch yang berbeda?

Inilah gunanya merge.

### Konsep Merge
**git merge** digunakan untuk menggabungkan perubahan dari satu branch ke branch lain.
Perintah ini membuat sebuah commit spesial yang memiliki dua “orang tua” (parent commits).

Commit ini dapat diartikan sebagai:
> “Gabungkan semua perubahan dari commit A dan commit B, beserta seluruh riwayat commit yang menjadi leluhur keduanya.”

Bayangin:
- Branch = jalan yang bercabang dari satu titik.
- Merge = bikin jalur yang menyatukan dua cabang jadi satu jalan lagi.

### visual
contoh kasus:
Kita membuat branch baru bernama bugFix lalu membuat commit di dalamnya:

```bash
git branch bugFix
git checkout bugFix
git commit
```

![alt text](<images/1_Introduction Sequence/image-4.png>)

Kemudian kita kembali ke branch main dan juga membuat commit baru:

```bash
git checkout main
git commit
```

Sekarang kita ingin menggabungkan perubahan dari bugFix ke main.
Caranya cukup jalankan:

![alt text](<images/1_Introduction Sequence/image-5.png>)

```bash
git merge bugFix
```

![alt text](<images/1_Introduction Sequence/image-6.png>)

## 4. Git Rebase
Selain merge, ada cara lain untuk menggabungkan pekerjaan dari branch berbeda, yaitu rebase.

#### Apa itu Rebase?

Rebase memindahkan serangkaian commit dari satu branch, lalu “menempelkan” (copy & paste) commit-commit tersebut di atas branch lain.

Bedanya dengan merge:
- Merge → menggabungkan riwayat dari dua branch menjadi satu, tapi tetap mempertahankan bentuk cabang.
- Rebase → membuat riwayat commit menjadi lurus (linear), seolah semua commit dibuat di satu jalur yang sama.

#### Kenapa Rebase Berguna?
- Membuat riwayat commit lebih rapi dan mudah dibaca.
- Tidak ada commit “gabungan” khusus seperti merge commit.

### visual
Misalnya kita punya branch bugFix yang sudah berisi commit baru, dan kita ingin menaruhnya di atas branch main.

```bash
git checkout -b bugFix
git commit
git checkout main
git commit
```

![alt text](<images/1_Introduction Sequence/image-7.png>)

Pindah ke branch yang mau di-rebase:
```bash
git checkout bugFix
```

Rebase ke main:
```bash
git rebase main
```

![alt text](<images/1_Introduction Sequence/image-8.png>)
