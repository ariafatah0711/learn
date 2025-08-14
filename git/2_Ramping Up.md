# Ramping Up
## 1. Moving around in Git
Sebelum masuk ke fitur-fitur Git yang lebih lanjut, penting untuk paham cara berpindah-pindah di dalam commit tree proyek kita.
Kalau sudah terbiasa, perintah Git lain akan terasa jauh lebih mudah.

#### Apa itu HEAD?
**HEAD** adalah nama simbolik untuk commit yang sedang kita gunakan saat ini — bisa dibilang, “posisi kerja” kita di Git.
- HEAD biasanya menunjuk ke sebuah branch (misalnya main atau bugFix).
- Saat kita membuat commit baru, HEAD (dan branch yang ditunjuknya) akan pindah ke commit tersebut

Contoh:
```
HEAD -> main -> C1
```
> Artinya kita sedang berada di branch main pada commit C1.

#### Apa itu Detaching HEAD?
Detached HEAD berarti HEAD menunjuk langsung ke commit tertentu, bukan ke sebuah branch.

Contoh perubahan:
```
Sebelum: HEAD -> main -> C1
Sesudah: HEAD -> C1
```

Kita masih bisa melihat isi commit tersebut, tapi perubahan yang kita buat tidak akan tersimpan di branch manapun kecuali kita membuat branch baru.

### visual
![alt text](<images/2_Ramping Up/image.png>)

Misalnya kita ingin berpindah ke commit C4:

```bash
git checkout C4
```

![alt text](<images/2_Ramping Up/image-1.png>)

---

## 2. Relative Refs
Menentukan commit dengan hash panjang memang merepotkan. Di dunia nyata, hash commit bisa sangat panjang, misalnya:

```
fed2da64c0efc5293610bdd892f82a58e8cbc5d8
```

Untungnya, Git cukup pintar — kita hanya perlu mengetik sebagian hash selama sudah unik, misalnya:

```bash
git checkout fed2
```

#### Apa itu Relative Refs?
Daripada selalu mengetik hash, kita bisa berpindah relatif dari posisi yang sudah kita ingat, seperti HEAD atau nama branch.

Dua operator dasar yang sering dipakai:
1. Caret (^)
   - HEAD^ → commit sebelum HEAD (parent pertama).
   - HEAD^^ → commit dua langkah sebelum HEAD.
   > Bisa juga dengan nama branch: main^ → parent dari commit terakhir di branch main.
2. Tilde (~)
   - Bentuk singkat untuk melompat beberapa langkah sekaligus.
   > Contoh: HEAD~3 artinya lompat 3 commit ke belakang dari HEAD.

### visual
<!-- misalnya kita Pindah ke commit tertentu:
```bash
git checkout C4
```
atau ke branch:
```bash
git checkout bugFix
```
![alt text](<images/2_Ramping Up/image-2.png>) -->

Pindah relatif ke commit sebelumnya dengan Caret(^):
```bash
git checkout HEAD^
git checkout bugFix^
```
![alt text](<images/2_Ramping Up/image-3.png>)

---

## 3. The "~" operator
Kalau kita mau mundur beberapa commit sekaligus, mengetik ^ berulang kali bisa melelahkan.
Nah, Git menyediakan tilde (~) operator untuk mempersingkatnya.

Cara Kerja ~:
- HEAD~3 → mundur 3 commit dari posisi HEAD sekarang.
- main~2 → mundur 2 commit dari commit terakhir di branch main.
- Jika tidak ada angka (HEAD~), artinya sama seperti HEAD^ (mundur 1 commit).

Contoh:
```bash
git checkout HEAD~1  # Sama seperti HEAD^
git checkout HEAD~5  # Mundur 5 commit
```

#### Branch Forcing dengan -f
Selain untuk navigasi, relative refs juga berguna untuk memindahkan posisi branch secara cepat.

```bash
git branch -f <nama_branch> <target_commit>
```

> -f berarti force, memindahkan branch ke commit tertentu meskipun ada riwayat yang hilang.

Biasanya kita pakai relative refs sebagai target commit.
```bash
git branch -f main HEAD~3  # Pindahkan branch main mundur 3 commit dari HEAD
```

> ⚠️ Di Git asli, kita tidak bisa memindahkan branch yang sedang kita gunakan.

### visual
![alt text](<images/2_Ramping Up/image-4.png>)

```bash
git branch -f main C6
git branch -f bugFix C0
git checkout HEAD~1
```

![alt text](<images/2_Ramping Up/image-5.png>)

---

## 4. Reversing Changes in Git
Di Git, ada beberapa cara untuk membatalkan perubahan.
Kita akan fokus pada cara membatalkan di level tinggi — bagaimana perubahan di-undo, bukan detail kecil seperti staging.

Dua perintah utama untuk membatalkan perubahan adalah:

#### Git Reset
- Digunakan untuk memundurkan posisi branch ke commit sebelumnya.
- Seolah commit tertentu tidak pernah dibuat.
- Cocok untuk branch lokal yang belum dibagikan ke orang lain.
- Ini adalah bentuk rewriting history (mengubah riwayat).

Contoh:
```bash
git reset HEAD~1    # Mundur 1 commit dari HEAD
git reset local~1   # Mundur 1 commit dari branch 'local'
```
> Hasilnya: branch akan kembali ke commit sebelumnya, dan commit yang dihapus tidak ada lagi di riwayat branch.

#### Git Revert
Digunakan untuk membatalkan commit dengan membuat commit baru yang berisi kebalikan dari perubahan tersebut.

Aman digunakan pada branch remote yang dipakai bersama tim, karena tidak mengubah riwayat yang sudah dibagikan.

| Perintah     | Cara Kerja                        | Aman untuk Remote? | Mengubah Riwayat? |
| ------------ | --------------------------------- | ------------------ | ----------------- |
| `git reset`  | Memundurkan branch ke commit lama | ❌ Tidak aman       | ✅ Ya              |
| `git revert` | Membuat commit pembatalan         | ✅ Aman             | ❌ Tidak           |


### visual
![alt text](<images/2_Ramping Up/image-6.png>)

kita lakukan reset pada branch local agar
```bash
git reset local~1
```
atau gunakan reset HEAD karena saat ini kita sedang berada di branch local
```bash
git reset HEAD~1
```

![alt text](<images/2_Ramping Up/image-7.png>)

setelah itu kita lakukan checkout ke branch pushed
```bash
git checkout pushed
```
dan lakukan revert di commit tertentu
```bash
git revert C2
```
atau
```bash
git revert pushed
```

![alt text](<images/2_Ramping Up/image-8.png>)
