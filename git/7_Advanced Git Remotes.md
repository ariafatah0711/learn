# Advanced Git Remotes
## 1. Merging feature branches
Pada proyek besar, developer biasanya mengerjakan fitur di feature branch terpisah (berdasarkan branch main), lalu baru menggabungkannya ke main saat fitur sudah siap.

Prinsipnya mirip seperti pelajaran sebelumnya, di mana branch sampingan (side branch) juga di-push ke remote. Namun, kali ini kita tambahkan satu langkah ekstra:

Beberapa developer memilih hanya melakukan push dan pull saat berada di branch main, supaya main selalu sinkron dengan versi di remote (o/main).

Di workflow ini kita akan menggabungkan dua konsep:
- Mengintegrasikan pekerjaan dari feature branch ke main
- Melakukan sinkronisasi dengan remote (push dan pull)

### visual
![alt text](<images/7_Advanced Git Remotes/image.png>)

- Terdapat tiga feature branch: side1, side2, dan side3.
- Kita ingin meng-push ketiga branch tersebut ke remote secara berurutan.
- Remote sudah diperbarui, sehingga kita juga perlu menggabungkan perubahan terbaru dari o/main ke pekerjaan kita.

#### Ambil pembaruan dari remote
```bash
git fetch
```
![alt text](<images/7_Advanced Git Remotes/image-1.png>)

#### Rebase setiap branch secara berurutan
Gabungkan o/main ke side1:
```bash
git rebase o/main side1 # o/main <= side1
```
![alt text](<images/7_Advanced Git Remotes/image-2.png>)

Gabungkan side2 ke side1, side3 ke side1, dan main ke side3
```bash
git rebase side1 side2 # side1 <= side2
git rebase side2 side3 # side2 <= side3
git rebase side3 main # side3 <= main
```
![alt text](<images/7_Advanced Git Remotes/image-3.png>)
![alt text](<images/7_Advanced Git Remotes/image-4.png>)

#### Kirim hasil akhirnya ke remote
```bash
git push
```
![alt text](<images/7_Advanced Git Remotes/image-5.png>)

> Rebase menjaga riwayat commit tetap bersih dan linear.
Urutan rebase sangat penting di sini, karena setiap branch membangun fitur dari branch sebelumnya.
Pastikan tidak ada konflik yang tertinggal sebelum melakukan push.

---

## 2. Merging with remote
Untuk mendorong (push) pembaruan ke remote, kita hanya perlu menggabungkan perubahan terbaru dari remote (o/main) ke branch lokal.
Ada dua cara: merge atau rebase.

Kenapa sebelumnya fokus ke rebase?
- Kelebihan rebase: Riwayat commit bersih dan linear.
- Kekurangan rebase: Mengubah urutan riwayat commit (history rewriting).

> Sebagian developer memilih merge untuk mempertahankan riwayat asli, sebagian lainnya lebih suka rebase untuk tampilan commit tree yang rapi.
📌 Pada level ini, kita coba menggunakan merge seperti pada latihan sebelumnya.
### visual
![alt text](<images/7_Advanced Git Remotes/image-7.png>)

Cara 1 — Merge langsung di main
```bash
git checkout main
git pull
```
![alt text](<images/7_Advanced Git Remotes/image-6.png>)

```bash
git merge side1
git merge side2
git merge side3
git push
```
![alt text](<images/7_Advanced Git Remotes/image-8.png>)

#### cara gw tapi agak lama
```bash
git fetch
git checkout o/main
# git merge side1
# git merge side2
# git merge side3
git merge side1 side2 side3
git branch -f main

git checkout main
git push
```

---

## 3. Remote Tracking
Remote-tracking branch adalah mekanisme yang menghubungkan branch lokal dengan branch di remote.
Misalnya, main biasanya tracking ke o/main.
Koneksi ini membuat Git tahu:
- Saat pull → merge target default adalah o/main.
- Saat push → tujuan default adalah main di remote.
> Saat git clone, Git otomatis membuat branch lokal main yang tracking ke o/main.

#### Membuat branch tracking remote
Cara 1 – Saat membuat branch
```bash
git checkout -b side o/main
```
> Membuat branch lokal side yang tracking ke o/main.

Cara 2 – Mengatur tracking pada branch yang sudah ada
```bash
git branch -u o/main side
```
Jika branch side sedang aktif, cukup:
```bash
git branch -u o/main
```

### visual
![alt text](<images/7_Advanced Git Remotes/image-11.png>)

Kita akan membuat branch side yang tracking o/main, lalu meng-push ke remote main meskipun lokalnya bukan di branch main.
```bash
git checkout -b side o/main

git commit
```
![alt text](<images/7_Advanced Git Remotes/image-9.png>)

```bash
git pull --rebase
git push
```
![alt text](<images/7_Advanced Git Remotes/image-10.png>)

---

## 4. Git push arguments
Pada pelajaran sebelumnya, kita tahu git push tanpa argumen akan menggunakan remote tracking branch untuk menentukan ke mana perubahan dikirim.
Namun, kita juga bisa memberi argumen agar Git tahu persis:
```bash
git push <remote> <place>
```

- <remote> → nama remote, biasanya origin
- <place> → sumber & tujuan commit (branch)

contoh:
```bash
git push origin main
```

Artinya: ambil commit dari branch lokal main → kirim ke branch main di remote origin.

> 📌 Karena kita sudah menentukan semuanya, posisi branch lokal saat ini tidak berpengaruh.
Kalau HEAD sedang di branch yang tidak tracking remote dan kita jalankan git push tanpa argumen → perintah akan gagal.

#### visual
![alt text](<images/7_Advanced Git Remotes/image-12.png>)

Kita ingin memperbarui dua branch di remote (main dan foo) tanpa melakukan git checkout.
```bash
git push origin main
git push origin foo
```

![alt text](<images/7_Advanced Git Remotes/image-13.png>)

---

## 5. Git push arguments -- Expanded!
