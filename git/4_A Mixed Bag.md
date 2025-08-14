# A Mixed Bag
## 1. Locally stacked commits
Bayangkan situasinya seperti ini:
Kamu sedang memburu sebuah bug yang susah banget dilacak.
Untuk membantu investigasi, kamu menambahkan beberapa perintah debug dan print statement di berbagai titik kode.
Setiap perubahan kecil itu kamu commit supaya aman — dan akhirnya bug-nya ketemu juga! 🎉

Nah masalahnya: commit terakhir (bugFix) memang perlu masuk ke branch main, tapi commit debug-debug sebelumnya tidak boleh ikut.
Kalau kita langsung merge atau fast-forward, semua commit debug itu ikut masuk — dan itu jelas tidak diinginkan.

Solusinya: kita hanya pindahkan commit bugFix saja, tanpa commit debug.
Ada dua cara: interactive rebase atau cherry-pick.

### git rebase
Dengan interactive rebase, kita bisa memilih commit mana yang mau diambil atau dibuang sebelum memindahkan ke branch main.
![alt text](<images/4_A Mixed Bag/image.png>)

Mulai interactive rebase terhadap branch main
```bash
git rebase -i main
```
> Ini akan membuka daftar commit yang ada di bugFix tetapi belum ada di main.

```bash
drop C2
drop C3
pick C4
```

Setelah commit debug dihapus, jalankan rebase ke main
```bash
git rebase bugFix main
```
![alt text](<images/4_A Mixed Bag/image-1.png>)

# Sekarang branch main akan memiliki commit bugFix saja, tanpa commit debug.

### git chery-pick
Kalau kita sudah tahu hash commit bugFix, cara paling cepat adalah langsung memetik commit tersebut ke main.

Pindah ke branch main
```bash
git checkout main
```

Ambil commit bugFix (misalnya C4)
```bash
git cherry-pick C4
```

```bash
drop C2
drop C3
pick C4
```

---

## 2. Juggling Commits
Situasi ini cukup sering terjadi saat bekerja dengan Git. Misalnya, kita memiliki dua commit yang berurutan:
- newImage – menambahkan gambar baru
- caption – menambahkan caption

Tiba-tiba, tim desain meminta perubahan kecil pada dimensi gambar di commit newImage, padahal commit tersebut tidak berada di posisi terakhir di riwayat.

Untuk mengatasinya, kita bisa menggunakan teknik git rebase -i untuk mengatur ulang urutan commit, lalu melakukan amend pada commit yang ingin diubah, kemudian mengembalikan urutan commit seperti semula.

### visual
![alt text](<images/4_A Mixed Bag/image-2.png>)

##### Reorder commit agar commit yang ingin diubah ada di paling atas
```bash
git rebase -i main
```
ubah urutanya
```bash
pick C3
pick C2
```
> Tujuannya agar C2 (newImage) berada di posisi terakhir sehingga bisa di-amend.
![alt text](<images/4_A Mixed Bag/image-3.png>)
![alt text](<images/4_A Mixed Bag/image-4.png>)

##### Amend commit yang sudah dipindahkan
Setelah newImage ada di atas, lakukan perubahan yang diminta lalu jalankan:
```bash
git commit --amend
```
![alt text](<images/4_A Mixed Bag/image-5.png>)

##### Kembalikan urutan commit seperti semula
```bash
git rebase -i main
```
ubah urutanya kembali
```bash
pick C2
pick C3
```
![alt text](<images/4_A Mixed Bag/image-6.png>)
![alt text](<images/4_A Mixed Bag/image-7.png>)

##### Pindahkan branch main ke hasil akhir
Setelah rebase selesai, arahkan main ke commit terbaru:
```bash
git rebase captions main
```
![alt text](<images/4_A Mixed Bag/image-8.png>)

---

## 3. Juggling Commits #2
Jika pada level sebelumnya kita menggunakan git rebase -i untuk memindahkan commit agar bisa di-amend, kali ini kita akan mencoba metode lain yang lebih sederhana dan mengurangi risiko konflik rebase, yaitu git cherry-pick.

Ingat, git cherry-pick berfungsi untuk menyalin commit dari mana saja di riwayat Git ke posisi HEAD saat ini (selama commit tersebut bukan nenek moyang HEAD).

### visual
![alt text](<images/4_A Mixed Bag/image-9.png>)

Checkout ke branch main
```bash
git checkout main
```

#### Cherry-pick commit C2 yang ingin diubah
Dengan ini, commit C2 akan ditempatkan di atas HEAD.
```bash
git cherry-pick C2
```
![alt text](<images/4_A Mixed Bag/image-10.png>)

Lakukan perubahan dan amend commit
```bash
git commit --amend
```
![alt text](<images/4_A Mixed Bag/image-11.png>)

##### Cherry-pick commit berikutnya (C3)
Setelah commit C2 diubah, tambahkan kembali commit yang sebelumnya berada setelahnya.
```bash
git cherry-pick C3
```
![alt text](<images/4_A Mixed Bag/image-12.png>)

---

## 4. Git Tags
Pada pelajaran sebelumnya, kita sudah tahu bahwa branch mudah dipindah-pindah, sering berubah, dan sifatnya sementara. Tapi bagaimana kalau kita ingin menandai titik penting dalam riwayat proyek secara permanen?

Misalnya:
- Menandai rilis versi mayor
- Menandai merge besar
- Menandai prototype atau milestone penting

Nah, di sinilah Git tags berperan.

> Tag berfungsi sebagai penanda permanen pada commit tertentu. Tidak seperti branch, tag tidak akan bergerak meskipun kita membuat commit baru. Kita juga tidak bisa meng-commit langsung di atas tag — jika kita checkout sebuah tag, kita akan berada di detached HEAD state.

### visual
![alt text](<images/4_A Mixed Bag/image-13.png>)

Kita tandai commit C1 sebagai versi awal (v0).
```bash
git tag v0 C1
```

Pertama, pindah ke C2:
```bash
git checkout C2
```

Lalu buat tag v1 di commit ini:
```bash
git tag v1
```

![alt text](<images/4_A Mixed Bag/image-14.png>)

> kita bisa juga pindah ke commit tertentu dengan checkout <tag>

Hasil akhirnya:
- v0 menunjuk ke commit C1
- v1 menunjuk ke commit C2
- Jika kita checkout v1, kita masuk detached HEAD karena tag tidak bisa digunakan untuk menulis commit baru.

---

## 5. Git Describe
Git describe adalah perintah untuk mengetahui posisi kita di commit tree relatif terhadap tag terdekat (yang berfungsi sebagai "anchor").

Perintah ini berguna ketika:
- Kita sedang mencari bug dengan git bisect
- Kita ingin tahu posisi commit saat bekerja di repo orang lain
- Kita ingin memberikan versi "sementara" berdasarkan tag terdekat

Bentuk umum perintah:
```bash
git describe <ref>
```
> <ref> bisa berupa nama branch, tag, hash commit, atau dibiarkan kosong (default: HEAD)

Format output:
```
<tag>_<numCommits>_g<hash>
```

Keterangan:
- tag → tag terdekat di riwayat commit
- numCommits → jumlah commit dari tag tersebut ke posisi saat ini
- hash → hash singkat commit saat ini

### visual
![alt text](<images/4_A Mixed Bag/image-16.png>)

```bash
git describe main
# v1_2_gC2
# Tag terdekat: v1
# Berjarak 2 commit dari tag v1
# Commit sekarang: hash dimulai C2

git describe side
git describe HEAD
```

![alt text](<images/4_A Mixed Bag/image-15.png>)
