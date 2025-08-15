# Git Remotes
## 1. Clone Intro
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

## 2. Remote Branches
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

## 3. Git Fetchin'
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

## 4. Git Pullin'
Kalau git fetch hanya mengambil data terbaru dari remote tanpa mengubah branch lokal, git pull adalah langkah lanjutannya: mengambil data + langsung menggabungkannya ke branch lokal.

#### Cara Kerja
Sebenarnya git pull hanyalah shorthand dari:
```bash
git fetch
git merge <remote>/<branch>
```

sama artinya dengan:
```bash
git pull
```

#### Alternatif Selain Merge
Setelah fetch, kita bisa menggabungkan perubahan dari remote dengan cara lain, misalnya:
- **git cherry-pick o/main** → ambil commit tertentu.
- **git rebase o/main** → susun ulang commit di atas commit remote.
- **git merge o/main** → gabungkan semua perubahan remote.
> git pull hanya otomatis memilih metode merge secara default.

### visual
Misalnya remote punya commit baru C3:
![alt text](<images/6_Git Remotes/image-4.png>)

lakukan fetch
```bash
git fetch
```
![alt text](<images/6_Git Remotes/image-5.png>)

lakukan merge
```bash
git merge o/main
```

![alt text](<images/6_Git Remotes/image-6.png>)

atau kita singkat menggunakan ini
```bash
git pull
```

kalo gunaian git fetch + git rebase
```bash
git fetch
git rebase o/main
```
![alt text](<images/6_Git Remotes/image-7.png>)

kalo gunaian git fetch + git cherry-pick
```bash
git fetch
git cherry-pick o/main
```

![alt text](<images/6_Git Remotes/image-8.png>)

---

## 5. Simulating collaboration
Di dunia nyata, kita sering harus mengambil perubahan dari remote yang dibuat oleh orang lain (rekan kerja, teman, atau kolaborator).
Di simulasi ini, kita bisa memalsukan kondisi tersebut menggunakan perintah khusus:

```bash
git fakeTeamwork
```

Cara Kerja git fakeTeamwork
- Tanpa argumen → menambahkan 1 commit baru di remote branch main.
- Dengan argumen angka → menambahkan sejumlah commit baru di branch main.
- Dengan argumen angka + nama branch → menambahkan sejumlah commit baru di branch tertentu.

Contoh:
```bash
git fakeTeamwork 2 # → menambahkan 2 commit baru di main.
git fakeTeamwork 3 foo # menambahkan 3 commit baru di branch foo
```

### visual
Buat remote (dengan git clone di simulasi ini).

```bash
git clone
```

![alt text](<images/6_Git Remotes/image-9.png>)

Buat commit palsu di remote (misalnya 2 commit di main), dan Buat commit lokal di branch main:
```bash
git fakeTeamwork 2
git commit
```

![alt text](<images/6_Git Remotes/image-10.png>)

Ambil perubahan remote dan gabungkan:

cara 1: Fetch + Merge
```bash
git fetch
git merge o/main
```

Cara 2: Pull langsung
```bash
git pull
```

![alt text](<images/6_Git Remotes/image-11.png>)

> git fakeTeamwork di sini hanyalah alat simulasi untuk mempelajari workflow kolaborasi:
> Remote di-update oleh orang lain → kita perlu fetch atau pull untuk menyelaraskan pekerjaan.

---

## 6. Git Push
Kalau git pull digunakan untuk mengambil perubahan dari remote ke lokal, maka git push adalah kebalikannya: mengirim perubahan dari lokal ke remote.

#### Fungsi git push
- Mengunggah commit dari branch lokal ke branch di remote.
- Memperbarui branch di remote agar sesuai dengan commit terbaru dari lokal.
- Setelah push selesai, semua orang yang terhubung ke remote bisa mengambil (pull/fetch) perubahan tersebut.
> Anggap saja git push sebagai perintah untuk mempublikasikan pekerjaanmu.

> Perilaku git push tanpa argumen dapat berbeda tergantung pada konfigurasi Git (push.default).
Di sini kita memakai mode upstream, artinya git push akan mengirim branch ke remote branch yang dilacak (tracked branch).

### visual
Buat commit di lokal
```bash
git commit
git commit
```
![alt text](<images/6_Git Remotes/image-12.png>)

> Branch main di lokal sekarang lebih maju dibandingkan remote.

Kirim perubahan ke remote
```bash
git push
```

![alt text](<images/6_Git Remotes/image-13.png>)

> Commit baru diunggah ke remote, branch main di remote ikut maju, dan o/main (representasi remote di lokal) ikut diperbarui.

---

## 7. Diverged Work
Selama ini kita sudah belajar cara pull commit orang lain dan push perubahan kita sendiri.
Kedengarannya sederhana, tapi akan jadi rumit kalau riwayat repository mulai bercabang.

Contoh kasus
- Senin: kamu clone repository dan mulai membuat fitur baru.
- Jumat: fitur selesai, siap di-push.
- Masalahnya: rekan kerja sudah menambahkan banyak perubahan di main selama kamu bekerja.
- Commit kamu sekarang berdasarkan versi lama dari main.

Jika kamu langsung git push, Git bingung:
- Apakah harus mengembalikan repo remote ke kondisi Senin?
- Menambahkan kodenya tanpa menghapus perubahan baru?
- Mengabaikan commit kamu?

> Karena kondisinya ambigu, Git menolak push dan memaksa kamu menggabungkan perubahan terbaru dari remote terlebih dahulu.

#### Solusi: Rebase atau Merge
```bash
git fetch
git rebase o/main
git push

git fetch
git merge o/main
git push

git pull # use merge
git push

git pull --rebase
git push
```

### visual
```bash
git clone
```
```bash
git fakeTeamwork
git commit
```

![alt text](<images/6_Git Remotes/image-14.png>)

cara 1 Rebase (Direkomendasikan di kasus ini):
> Rebase akan memindahkan commit kamu agar berdasar pada commit terbaru dari remote.
```bash
git fetch
git rebase o/main
git push
```

Atau versi singkatnya:
```bash
git pull --rebase
git push
```

> 📌 Kelebihan: Riwayat commit tetap rapi (linear).

![alt text](<images/6_Git Remotes/image-16.png>)

cara 2 (cuma ini gak bakal compleate karena kita disuruh yang rebase):
> Merge akan membuat merge commit yang menggabungkan pekerjaan lokal dan remote.
```bash
git fetch
git merge o/main
git push
```
Atau versi singkatnya:
```bash
git pull # use merge
git push
```

![alt text](<images/6_Git Remotes/image-15.png>)

> 📌 Kelebihan: Tidak mengubah commit yang sudah ada.
📌 Kekurangan: Riwayat commit bisa jadi lebih berantakan.

---

## 8. Remote Rejected!
Jika kamu bekerja di tim kolaborasi besar, biasanya branch main akan dikunci dan hanya bisa di-update melalui proses Pull Request.

Kalau kamu tidak sengaja commit langsung ke main di lokal dan mencoba push, kemungkinan besar akan muncul pesan seperti ini:
```bash
! [remote rejected] main -> main (TF402455: Pushes to this branch are not permitted; you must use a pull request to update this branch.)
```

#### Kenapa bisa ditolak?
Push tersebut ditolak karena remote repository memiliki kebijakan yang melarang push langsung ke branch main.
Sebagai gantinya, kamu harus membuat branch baru, lalu membuat Pull Request.

Namun, dalam kasus ini, kamu lupa dan terlanjur commit di main.
Solusinya:
- Buat branch baru (feature) dari commit yang sudah kamu buat.
- Push branch tersebut ke remote.
- Reset main agar kembali sinkron dengan remote supaya tidak menimbulkan konflik di kemudian hari.

### visual
#### cara 1
```bash
# Reset main agar sinkron dengan remote
git reset --hard o/main

# Pindah ke commit C2 (commit hasil kerja kamu)
git checkout C2

# Buat branch baru bernama feature dari commit C2
git checkout -b feature

Push branch feature ke remote
git push origin
```

#### Alternatif dengan git branch -f
![alt text](<images/6_Git Remotes/image-17.png>)

Paksa branch main agar mengarah ke commit origin/main:
```bash
git branch -f main o/main
```
![alt text](<images/6_Git Remotes/image-18.png>)

Buat branch baru dari commit C2:
```bash
git checkout -b feature C2
```
![alt text](<images/6_Git Remotes/image-19.png>)

Push branch tersebut ke remote:
```bash
git push origin
```
![alt text](<images/6_Git Remotes/image-20.png>)
